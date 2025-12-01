# Blogger Complete Technical Reference

*A definitive encyclopedia of Blogger's public URLs, API endpoints, feeds, template system, internal routes, and undocumented patterns.*

---

## 📋 Table of Contents

1. [Overview](#overview)
2. [Placeholder Conventions](#placeholder-conventions)
3. [Public URLs](#public-urls)
4. [URL Parameters](#url-parameters)
5. [Feed Endpoints (Atom/RSS)](#feed-endpoints-atomrss)
6. [Blogger API v3](#blogger-api-v3)
7. [Authentication & Authorization](#authentication--authorization)
8. [Media/Asset URLs](#mediaasset-urls)
9. [Internal Dashboard URLs](#internal-dashboard-urls)
10. [Comment System](#comment-system)
11. [Template Engine Data-Tags](#template-engine-data-tags)
12. [Widget & Integration URLs](#widget--integration-urls)
13. [Mobile & Specialized URLs](#mobile--specialized-urls)
14. [SEO & Metadata URLs](#seo--metadata-urls)
15. [Domain Behavior](#domain-behavior)
16. [Legacy URLs](#legacy-urls)
17. [Error Pages & Status](#error-pages--status)
18. [JavaScript API & Widgets](#javascript-api--widgets)
19. [Cross-Origin & Security](#cross-origin--security)
20. [Tools & Utilities](#tools--utilities)
21. [Known Limitations](#known-limitations)
22. [Contributing](#contributing)
23. [License](#license)

---

## 📖 Overview

This document is the **most comprehensive technical reference** for the Blogger platform, compiled from:

- **Official documentation** (where it exists)
- **Reverse engineering** of live Blogger instances
- **Community discoveries** from 15+ years of Blogger's existence
- **Internal testing** against actual Blogger endpoints
- **Historical archives** of deprecated but functional features

**Scope includes:**
- Public blog URLs (blogspot.com and custom domains)
- Internal dashboard routes (authenticated)
- Atom/RSS feed system (legacy but fully functional)
- Google Blogger API v3 (official REST API)
- Template engine (`data:` tags and `b:` elements)
- Comment system (anchors, parameters, feeds)
- Undocumented parameters that are confirmed to work
- Media serving patterns
- Mobile and AMP views
- SEO and metadata endpoints
- Legacy endpoints with backwards compatibility

---

## 🎯 Placeholder Conventions

All examples use these placeholders:

| Placeholder | Description | Example |
|-------------|-------------|---------|
| `{blogUrl}` | Blog hostname (no protocol) | `example.blogspot.com` |
| `{customDomain}` | Custom domain hostname | `example.com` |
| `{blogId}` | Numeric blog ID (API/internal) | `1234567890123456789` |
| `{postId}` | Numeric post ID | `9876543210987654321` |
| `{pageId}` | Numeric static page ID | `5555555555555555555` |
| `{postSlug}` | URL-safe post title | `my-awesome-post-title` |
| `{pageSlug}` | URL-safe page title | `about-me` |
| `{year}` | Four-digit year | `2024` |
| `{month}` | Two-digit month | `01` |
| `{label}` | Label/tag (URL-encoded) | `Technical%20Writing` |
| `{query}` | Search query (URL-encoded) | `python+tutorial` |
| `{commentId}` | Numeric comment ID | `1234567890` |
| `{userId}` | Google/Blogger user ID | `123456789012345678901` |
| `{profileId}` | Profile page ID | `12345678901234567890` |
| `{apiKey}` | Blogger API key | `AIzaSyB...` |
| `{clientId}` | OAuth 2.0 client ID | `123-abc.apps.googleusercontent.com` |
| `{imageId}` | Image identifier | `aHn2XyZ` |

---

## 🌐 Public URLs

### Blog Home

**Standard:** `https://{blogUrl}/`
**Mobile:** `https://{blogUrl}/?m=1`
**Print View:** `https://{blogUrl}/?view=print`

**Dynamic Views:**
- `?view=classic`      # Classic view
- `?view=flipcard`     # Flipcard view
- `?view=mosaic`       # Mosaic view
- `?view=sidebar`      # Sidebar view
- `?view=snapshot`     # Snapshot view
- `?view=timeslide`    # Timeslide view


### Posts

**Standard Format:**
`https://{blogUrl}/{year}/{month}/{postSlug}.html`

**With Parameters:**
- `?m=1`                          # Mobile version
- `?showComment={commentId}`      # Scroll to comment
- `#c{commentId}`                 # Comment anchor
- `?maxResults={n}`               # Limit comments
- `?sortBy=newest|oldest`         # Sort comments
- `?view=print`                   # Print view

**Example:**
`https://example.blogspot.com/2024/01/my-post.html?showComment=1234567890#c1234567890`


### Static Pages

**Format:** `https://{blogUrl}/p/{pageSlug}.html`
**Example:** `https://example.blogspot.com/p/about.html`


### Labels/Tags

**Format:** `https://{blogUrl}/search/label/{label}`

**Parameters:**
- `?max-results={n}`              # Posts per page (1-50)
- `?by-date=true|false`           # Sort by date
- `?updated-min={ISO-date}`       # Filter by update date
- `?updated-max={ISO-date}`       # Filter by update date
- `?m=1`                          # Mobile view

**Example:**
`https://example.blogspot.com/search/label/Tutorial?max-results=20&by-date=true`


### Search

**Format:** `https://{blogUrl}/search?q={query}`

**Parameters:**
- `&max-results={n}`              # Results per page
- `&by-date=true|false`           # Sort by date
- `&updated-min={ISO-date}`       # Date range filter
- `&updated-max={ISO-date}`       # Date range filter

**Example:**
`https://example.blogspot.com/search?q=python+tutorial&max-results=30`


### Archives

**Monthly Archive:**
`https://{blogUrl}/{year}/{month}/`

**Legacy Archive:**
`https://{blogUrl}/{year}_{month}_01_archive.html`

**Yearly Archive:**
`https://{blogUrl}/archive.html`


### Profile Pages

**Blogger Profile:**
`https://www.blogger.com/profile/{profileId}`

**Google Profile:**
`https://profiles.google.com/{userId}`


### Comment System

**Comment Form:**
`https://{blogUrl}/{year}/{month}/{postSlug}.html#comment-form`

**Post a Comment:**
`https://www.blogger.com/comment.g?blogID={blogId}&postID={postId}`

**Manage Comments:**
`https://www.blogger.com/comment-edit.g?blogID={blogId}&postID={postId}`


---

## ⚙️ URL Parameters

| Parameter | Value | Works On | Description |
|-----------|-------|----------|-------------|
| `m` | `1` | All pages | Mobile view |
| `view` | `classic`, `flipcard`, `mosaic`, `sidebar`, `snapshot`, `timeslide`, `print` | Home, Index | Force specific template/view |
| `showComment` | `{commentId}` | Posts | Scroll to and highlight comment |
| `maxResults` | `{number}` | Posts | Limit comments displayed |
| `max-results` | `{number}` | Labels, Search | Limit posts displayed |
| `sortBy` | `newest`, `oldest` | Posts | Sort comments |
| `by-date` | `true`, `false` | Labels, Search | Sort results by date |
| `updated-min` | `{ISO-date}` | Labels, Search | Filter by minimum update date |
| `updated-max` | `{ISO-date}` | Labels, Search | Filter by maximum update date |
| `alt` | `atom`, `rss`, `json` | Feeds | Feed format |
| `callback` | `{functionName}` | JSON feeds | JSONP callback |
| `start-index` | `{number}` | Feeds | Pagination start |
| `orderby` | `published`, `updated` | Feeds | Sort order |
| `q` | `{query}` | Search | Search query |

---

## 📡 Feed Endpoints (Atom/RSS)

### Posts Feeds

**Default (Atom):**
`https://{blogUrl}/feeds/posts/default`

**RSS Format:**
`https://{blogUrl}/feeds/posts/default?alt=rss`

**JSON Format:**
`https://{blogUrl}/feeds/posts/default?alt=json`

**JSONP:**
`https://{blogUrl}/feeds/posts/default?alt=json-in-script&callback=myCallback`

**By Label:**
`https://{blogUrl}/feeds/posts/default/-/{label}`


### Comments Feeds

**All Comments:**
`https://{blogUrl}/feeds/comments/default`

**Atom Format:**
`https://{blogUrl}/feeds/comments/default?alt=atom`

**Per Post:**
`https://{blogUrl}/feeds/{postId}/comments/default`

**Full Feed (with post content):**
`https://{blogUrl}/feeds/{postId}/comments/full`


### Archive Feeds

**By Year/Month:**
`https://{blogUrl}/feeds/{year}/{month}/posts/default`


### Search Feeds

**Search Results:**
`https://{blogUrl}/feeds/posts/default?q={query}`


---

## 🔌 Blogger API v3

Base URL: `https://www.googleapis.com/blogger/v3/`

### Authentication

**API Key:** `?key={apiKey}`
**OAuth 2.0:** `Authorization: Bearer {access_token}`


### Blogs

GET /blogs/{blogId}
GET /blogs/byurl?url={blogUrl}
GET /users/{userId}/blogs


### Posts

GET    /blogs/{blogId}/posts
GET    /blogs/{blogId}/posts/{postId}
GET    /blogs/{blogId}/posts/search?q={query}
POST   /blogs/{blogId}/posts
PUT    /blogs/{blogId}/posts/{postId}
PATCH  /blogs/{blogId}/posts/{postId}
DELETE /blogs/{blogId}/posts/{postId}

**Parameters:**
- fetchBodies=true|false
- fetchImages=true|false
- maxResults={number}
- pageToken={token}
- startDate={RFC-3339}
- endDate={RFC-3339}
- labels={label1,label2}
- status=draft|live|scheduled
- view=ADMIN|AUTHOR|READER


### Pages

GET    /blogs/{blogId}/pages
GET    /blogs/{blogId}/pages/{pageId}
POST   /blogs/{blogId}/pages
PUT    /blogs/{blogId}/pages/{pageId}
PATCH  /blogs/{blogId}/pages/{pageId}
DELETE /blogs/{blogId}/pages/{pageId}


### Comments

GET    /blogs/{blogId}/posts/{postId}/comments
GET    /blogs/{blogId}/posts/{postId}/comments/{commentId}
GET    /blogs/{blogId}/comments
GET    /blogs/{blogId}/comments/{commentId}
POST   /blogs/{blogId}/posts/{postId}/comments
DELETE /blogs/{blogId}/comments/{commentId}


### Users

GET /users/{userId}


### BlogUserInfos

GET /users/{userId}/blogs/{blogId}


### PostUserInfos

GET /users/{userId}/blogs/{blogId}/posts
GET /users/{userId}/blogs/{blogId}/posts/{postId}


### PageViews

GET /blogs/{blogId}/pageviews
GET /blogs/{blogId}/pageviews?range={today|week|month|all}


---

## 🔐 Authentication & Authorization

### OAuth 2.0 Scopes

**Full access:**
https://www.googleapis.com/auth/blogger

**Read-only:**
https://www.googleapis.com/auth/blogger.readonly


### OAuth URLs

**Authorization Endpoint:**
https://accounts.google.com/o/oauth2/auth
  ?client_id={clientId}
  &redirect_uri={redirectUri}
  &scope=https://www.googleapis.com/auth/blogger
  &response_type=code
  &access_type=offline
  &prompt=consent

**Token Endpoint:**
https://oauth2.googleapis.com/token

**Revoke Token:**
https://oauth2.googleapis.com/revoke?token={token}


### Dashboard Authentication

**Login:**
https://www.blogger.com/blogin.g?blogID={blogId}

**Logout:**
https://www.blogger.com/logout.g

**Switch Account:**
https://www.blogger.com/add.g?blogID={blogId}


---

## 🖼️ Media/Asset URLs

### Images

**Blogger-Hosted Images:**
https://blogger.googleusercontent.com/img/{blogId}/{imagePath}

**Post Images:**
https://{blogUrl}/images/p/{postId}/{filename}

**Resized Images:**
https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsE{hash}=s{size}

**Image Parameters:**
- `=s320`          # Width 320px
- `=w320-h240`     # Width 320px, height 240px
- `=s320-c`        # Cropped to square
- `=s0`            # Original size


### Icons & Assets

**Favicon:**
https://{blogUrl}/favicon.ico

**Blogger Logo:**
https://www.blogger.com/img/logo-{color}.png

**Static Assets:**
https://www.blogger.com/static/{version}/{asset}
  - v1/widgets/...
  - v1/css/...
  - v1/js/...


---

## 🛠️ Internal Dashboard URLs

### Posts Management

**All Posts:**
https://www.blogger.com/blogger.g?blogID={blogId}#allposts

**New Post:**
https://www.blogger.com/blogger.g?blogID={blogId}#editor

**Edit Post:**
https://www.blogger.com/blogger.g?blogID={blogId}#editor/target=post;postID={postId}

**Post Stats:**
https://www.blogger.com/post-stats.g?blogID={blogId}&postID={postId}


### Comments Management

**All Comments:**
https://www.blogger.com/blogger.g?blogID={blogId}#allcomments

**Awaiting Moderation:**
https://www.blogger.com/blogger.g?blogID={blogId}#comments-moderation


### Settings

**Basic Settings:**
https://www.blogger.com/blogger.g?blogID={blogId}#settings

**Posts & Comments:**
https://www.blogger.com/blogger.g?blogID={blogId}#settings-posts

**Email & Mobile:**
https://www.blogger.com/blogger.g?blogID={blogId}#settings-email

**Language & Formatting:**
https://www.blogger.com/blogger.g?blogID={blogId}#settings-language

**Search Preferences:**
https://www.blogger.com/blogger.g?blogID={blogId}#settings-search

**Custom Domain:**
https://www.blogger.com/blogger.g?blogID={blogId}#settings-domain


### Layout & Theme

**Template Editor:**
https://www.blogger.com/template-edit.g?blogID={blogId}

**Layout Editor:**
https://www.blogger.com/rearrange?blogID={blogId}&widgetType={type}

**Theme Gallery:**
https://www.blogger.com/template-gallery.g?blogID={blogId}

**Theme Preview:**
https://www.blogger.com/template-preview.g?blogID={blogId}


### Stats & Analytics

**Overview:**
https://www.blogger.com/stats.g?blogID={blogId}

**Traffic Sources:**
https://www.blogger.com/stats-traffic-sources.g?blogID={blogId}

**Audience:**
https://www.blogger.com/stats-audience.g?blogID={blogId}


---

## 💬 Comment System

### Anchors & Parameters

**Comment Anchor:**
#c{commentId}
  Example: #c1234567890123456789

**With Post URL:**
https://{blogUrl}/{year}/{month}/{postSlug}.html#c{commentId}

**Show Comment Parameter:**
?showComment={commentId}
  Combined: ?showComment=1234567890#c1234567890

**Comment Pagination:**
?commentPage={pageNumber}


### Comment Feeds by Type

**Recent Comments:**
https://{blogUrl}/feeds/comments/default

**By Post:**
https://{blogUrl}/feeds/{postId}/comments/default

**By Author:**
https://{blogUrl}/feeds/comments/default?author={email}


### Moderation Endpoints

**Approve Comment:**
https://www.blogger.com/comment-approve.g?blogID={blogId}&commentID={commentId}

**Delete Comment:**
https://www.blogger.com/comment-delete.g?blogID={blogId}&commentID={commentId}

**Mark as Spam:**
https://www.blogger.com/comment-spam.g?blogID={blogId}&commentID={commentId}


---

## 🎨 Template Engine Data-Tags

### Blog-Level Tags
xml
<!-- Basic Info -->
<data:blog.title/>                    <!-- Blog title -->
<data:blog.url/>                      <!-- Current page URL -->
<data:blog.homepageUrl/>              <!-- Blog homepage URL -->
<data:blog.pageTitle/>                <!-- Full page title -->
<data:blog.encoding/>                 <!-- Blog encoding -->

<!-- Metadata -->
<data:blog.metaDescription/>          <!-- Blog description -->
<data:blog.pageType/>                 <!-- Values: index, item, static_page, archive, label, search -->
<data:blog.languageDirection/>        <!-- ltr or rtl -->
<data:blog.isPrivate/>                <!-- true if private blog -->
<data:blog.searchLabel/>              <!-- Current search label -->
<data:blog.searchQuery/>              <!-- Current search query -->

<!-- Features -->
<data:blog.hasCustomDomain/>          <!-- true if custom domain -->
<data:blog.hasCustomLogo/>            <!-- true if custom logo -->
<data:blog.crosspostMessages/>        <!-- Cross-posting status -->

<!-- Feed Links -->
<data:blog.feedLinks/>                <!-- All available feeds -->
<data:blog.allPostsAtomFeedUrl/>      <!-- Atom feed URL -->
<data:blog.allPostsRssFeedUrl/>       <!-- RSS feed URL -->
<data:blog.allCommentsAtomFeedUrl/>   <!-- Comments Atom feed -->
<data:blog.allCommentsRssFeedUrl/>    <!-- Comments RSS feed -->

<!-- Stats -->
<data:blog.pageviewCount/>            <!-- Total pageviews -->
<data:blog.analyticsAccountNumber/>   <!-- Analytics ID -->


### Post-Level Tags
xml
<!-- Identification -->
<data:post.id/>                       <!-- Numeric post ID -->
<data:post.title/>                    <!-- Post title -->
<data:post.url/>                      <!-- Permanent post URL -->
<data:post.link/>                     <!-- Post link (same as url) -->

<!-- Content -->
<data:post.body/>                     <!-- Full post content -->
<data:post.snippet/>                  <!-- First part of content -->
<data:post.author/>                   <!-- Author display name -->
<data:post.authorDisplayName/>        <!-- Author name for display -->
<data:post.authorProfileUrl/>         <!-- Author profile URL -->
<data:post.authorPhotoUrl/>           <!-- Author photo URL -->

<!-- Dates -->
<data:post.timestamp/>                <!-- ISO 8601 timestamp -->
<data:post.dateHeader/>               <!-- Formatted date header -->
<data:post.published/>                <!-- Publication date -->
<data:post.updated/>                  <!-- Last update date -->

<!-- Metadata -->
<data:post.labels/>                   <!-- Array of labels -->
<data:post.labelsJson/>               <!-- Labels as JSON -->
<data:post.allowComments/>            <!-- true if comments allowed -->
<data:post.numComments/>              <!-- Number of comments -->
<data:post.showThreadedComments/>     <!-- true if threaded comments -->
<data:post.showReplyForm/>            <!-- true if reply form shown -->

<!-- Media -->
<data:post.firstImageUrl/>            <!-- First image in post -->
<data:post.featuredImage/>            <!-- Featured image URL -->
<data:post.thumbnailUrl/>             <!-- Thumbnail URL -->

<!-- Social -->
<data:post.sharePostUrl/>             <!-- URL for sharing -->
<data:post.sharePostTitle/>           <!-- Title for sharing -->
<data:post.backlinks/>                <!-- Incoming links -->

<!-- Ads -->
<data:post.includeAds/>               <!-- true if ads enabled -->
<data:post.adCode/>                   <!-- Ad code for this post -->


### Page-Level Tags
xml
<data:page.title/>                    <!-- Page title -->
<data:page.url/>                      <!-- Page URL -->
<data:page.id/>                       <!-- Numeric page ID -->
<data:page.body/>                     <!-- Page content -->
<data:page.timestamp/>                <!-- Creation timestamp -->
<data:page.showMobileStyle/>          <!-- true if mobile style -->


### Comment-Level Tags
xml
<data:comment.id/>                    <!-- Comment ID -->
<data:comment.body/>                  <!-- Comment content -->
<data:comment.author/>                <!-- Author name -->
<data:comment.authorUrl/>             <!-- Author URL -->
<data:comment.authorPhotoUrl/>        <!-- Author photo -->
<data:comment.timestamp/>             <!-- Comment date -->
<data:comment.deleteUrl/>             <!-- Delete URL (admin only) -->
<data:comment.isAdmin/>               <!-- true if admin comment -->
<data:comment.isOwner/>               <!-- true if blog owner -->
<data:comment.isSpam/>                <!-- true if marked spam -->


### Widget & Layout Tags
xml
<!-- Section -->
<data:section.id/>                    <!-- Section ID -->
<data:section.class/>                 <!-- CSS class -->
<data:section.name/>                  <!-- Section name -->
<data:section.description/>           <!-- Section description -->
<data:section.showAd/>                <!-- true if ads shown -->

<!-- Widget -->
<data:widget.instanceId/>             <!-- Widget instance ID -->
<data:widget.sectionId/>              <!-- Parent section ID -->
<data:widget.type/>                   <!-- Widget type -->
<data:widget.title/>                  <!-- Widget title -->

<!-- Messages -->
<data:messages.aboutThisBlog/>        <!-- "About this blog" -->
<data:messages.comments/>             <!-- "Comments" -->
<data:messages.postAComment/>         <!-- "Post a Comment" -->
<data:messages.postComment/>          <!-- "Post Comment" -->
<data:messages.subscribeTo/>          <!-- "Subscribe to" -->
<data:messages.viewBlog/>             <!-- "View blog" -->


---

## 🧩 Widget & Integration URLs

### Follow by Email (FeedBurner)

**Subscription:**
https://feedburner.google.com/fb/a/mailverify?uri={feedName}&loc=en_US

**FeedBurner Stats:**
https://feedburner.google.com/fb/a/stats/feed/{feedUri}


### Social Sharing

**Share to Blogger:**
https://www.blogger.com/share-post.g
  ?blogID={blogId}
  &postID={postId}
  &target={targetBlogId}

**Email This:**
mailto:?subject={title}&body={url}

**Add to Google+:**
https://plus.google.com/share?url={url}


### Gadgets & Badges

**Blogger Badge:**
https://www.blogger.com/badge.g?blogID={blogId}

**Followers Gadget:**
https://www.blogger.com/followers.g?blogID={blogId}

**Newsreel Gadget:**
https://www.blogger.com/newsreel.g


---

## 📱 Mobile & Specialized URLs

### Mobile Views

**Standard Mobile:**
https://{blogUrl}/?m=1

**Mobile Template:**
https://{blogUrl}/?m=1&app=0

**In-App Browser:**
https://{blogUrl}/?m=1&app=1


### AMP (Accelerated Mobile Pages)

**AMP Version:**
https://{blogUrl}/{year}/{month}/{postSlug}.amp.html

**AMP HTML:**
https://{blogUrl}/amp/{year}/{month}/{postSlug}

**AMP Style:**
https://{blogUrl}/amp_css/{timestamp}


### Print Views

**Print-Friendly:**
https://{blogUrl}/{year}/{month}/{postSlug}.html?view=print

**Print CSS:**
https://{blogUrl}/print.css


### App Deep Links

**Android App:**
intent://{blogUrl}/#Intent;package=com.google.android.apps.blogger;end

**iOS App:**
blogger://{blogUrl}/

**Edit in App:**
blogger://edit?blogId={blogId}&postId={postId}


---

## 🔍 SEO & Metadata URLs

### Sitemaps

**Main Sitemap:**
https://{blogUrl}/sitemap.xml

**Posts Sitemap:**
https://{blogUrl}/sitemap-posts.xml

**Pages Sitemap:**
https://{blogUrl}/sitemap-pages.xml

**Labels Sitemap:**
https://{blogUrl}/sitemap-labels.xml

**Archive Sitemap:**
https://{blogUrl}/sitemap-archive.xml


### Robots.txt

**Standard:**
https://{blogUrl}/robots.txt

**Custom:**
https://{blogUrl}/robots.txt?t={timestamp}


### Search Metadata

**OpenSearch Description:**
https://{blogUrl}/opensearch.xml

**Manifest:**
https://{blogUrl}/manifest.json

**Web App Manifest:**
https://{blogUrl}/blogger.webmanifest


### Structured Data

**Blog JSON-LD:**
https://{blogUrl}/feeds/posts/default?alt=json&callback=bloggerJSONCallback


---

## 🌍 Domain Behavior

### Domain Types

**Blogspot Domain:**
https://{blogName}.blogspot.com/

**Country-Specific Blogspot:**
https://{blogName}.blogspot.{tld}/
  Examples: .co.uk, .fr, .de, .jp

**Custom Domain:**
https://{customDomain}/

**HTTPS Enforcement:**
- HTTP → HTTPS automatic redirect
- HSTS preload for blogspot.com


### Canonical URLs

**Canonical Tag:**
<link rel="canonical" href="https://{blogUrl}/{year}/{month}/{postSlug}.html" />

**Alternate URLs:**
<link rel="alternate" type="application/atom+xml" href="...">
<link rel="alternate" type="application/rss+xml" href="...">
<link rel="alternate" media="only screen and (max-width: 640px)" href="...">


### Redirect Patterns

**WWW Handling:**
- https://www.{blogUrl}/ → https://{blogUrl}/ (or vice versa based on settings)

**Trailing Slash:**
- /post → /post/ (normalized)

**Case Sensitivity:**
- URLs are case-insensitive for blogspot.com
- Custom domains follow server configuration


---

## 🗃️ Legacy URLs

### AtomPub API (Deprecated but Functional)

**Posts:**
https://www.blogger.com/feeds/{blogId}/posts/default

**Comments:**
https://www.blogger.com/feeds/{blogId}/{postId}/comments/default

**With Auth:**
https://{username}:{passwd}@www.blogger.com/feeds/{blogId}/posts/default


### Old Comment System

**Post Comment:**
https://www.blogger.com/comment.g?blogID={blogId}&postID={postId}

**Popup Comment:**
https://www.blogger.com/comment-iframe.g?blogID={blogId}&postID={postId}


### Export & Backup

**Export Blog:**
https://www.blogger.com/blog-export.g?blogID={blogId}

**Import Blog:**
https://www.blogger.com/blog-import.g

**Backup Template:**
https://www.blogger.com/template-backup.g?blogID={blogId}


### Old Template System

**Classic Template:**
https://{blogUrl}/?action=template&widgetType=BlogArchive&widgetId=...

**Layouts Template:**
https://www.blogger.com/layouts-edit.g?blogID={blogId}


---

## ⚠️ Error Pages & Status

### HTTP Status Codes

**200 OK** - Normal response
**301 Moved Permanently** - Redirect (domain change, canonical)
**302 Found** - Temporary redirect (mobile view, login)
**404 Not Found** - Page doesn't exist
**500 Internal Server Error** - Blogger system error
**503 Service Unavailable** - Maintenance, rate limiting


### Error Pages

**404 Page:**
https://{blogUrl}/404.html

**Maintenance:**
https://{blogUrl}/maintenance.html

**Access Denied:**
https://www.blogger.com/access-denied.g

**Blog Not Found:**
https://www.blogger.com/blog-not-found.g


### System Status

**Status Page:**
https://www.blogger.com/status.g

**Known Issues:**
https://www.blogger.com/known-issues.g

**Outage Report:**
https://www.blogger.com/outage-report.g


---

## 📊 JavaScript API & Widgets

### JSON Feeds

**Posts JSON:**
https://{blogUrl}/feeds/posts/default?alt=json

**With Callback:**
https://{blogUrl}/feeds/posts/default?alt=json-in-script&callback=myFunction

**Search Results JSON:**
https://{blogUrl}/feeds/posts/default?alt=json&q={query}

**Comments JSON:**
https://{blogUrl}/feeds/comments/default?alt=json


### Dynamic Views JSON

**Posts for View:**
https://www.blogger.com/feeds/{blogId}/posts/default
  ?alt=json
  &callback=__blogger_dynamic_views__.__callbacks__.posts
  &path={path}


### Widget JavaScript

**Blogger JS Library:**
https://www.blogger.com/static/v1/jsbin/{filename}.js

**Widget Renderer:**
https://www.blogger.com/render/{widgetType}?blogID={blogId}

**Gadget JS:**
https://www.blogger.com/gadgets/js/{gadgetId}


---

## 🛡️ Cross-Origin & Security

### CORS Headers

**Feeds:**
Access-Control-Allow-Origin: *

**API:**
Access-Control-Allow-Origin: https://www.blogger.com


### Content Security Policy

**Default CSP:**
default-src 'self' https://www.blogger.com
img-src * data: https:
script-src 'unsafe-inline' 'unsafe-eval' https: http:
style-src 'unsafe-inline' https: http:


### Security Headers

**Blogspot.com:**
X-Content-Type-Options: nosniff
X-XSS-Protection: 1; mode=block
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload


---

## 🛠️ Tools & Utilities

### Validation Tools

**Template Validator:**
https://www.blogger.com/template-validator.g

**Feed Validator:**
https://www.blogger.com/feeds/validate

**HTML Validator:**
https://www.blogger.com/html-validator.g


### Development Tools

**API Explorer:**
https://developers.google.com/apis-explorer/#p/blogger/v3/

**OAuth Playground:**
https://developers.google.com/oauthplayground/

**API Console:**
https://console.developers.google.com/apis/api/blogger.googleapis.com


### Diagnostic URLs

**Blog Info:**
https://{blogUrl}/?d=info

**Debug View:**
https://{blogUrl}/?d=debug

**Show Variables:**
https://{blogUrl}/?showvars=1


---

## ⚠️ Known Limitations

### API Limitations

**Rate Limits:**
- 500 requests per 100 seconds per user (OAuth)
- 10,000 requests per 100 seconds per project (API key)

**Quotas:**
- 50 posts per day (create/update/delete)
- 500 comments per day
- 50 pages per day

**Size Limits:**
- Post body: 1MB
- Image upload: 20MB
- Comment body: 4096 characters


### Platform Limitations

**Slug Generation:**
- Post slugs auto-generated from title
- Cannot be manually specified via API
- Limited to 128 characters

**Label Limits:**
- 2000 labels per blog
- 20 labels per post
- Label length: 225 characters max

**Comment System:**
- Threaded comments depth: 5 levels
- Comments per post: Unlimited in practice
- Comment moderation: 30 days auto-close


### Browser Compatibility

**Supported Browsers:**
- Chrome (latest 2 versions)
- Firefox (latest 2 versions)
- Safari (latest 2 versions)
- Edge (latest 2 versions)

**IE Compatibility:**
- IE 11 (limited support)
- Dynamic views not supported in IE


---

## 🤝 Contributing

Found something missing or incorrect?

1. **Fork** this repository
2. **Test** the URL/endpoint thoroughly
3. **Document** with:
   - Complete URL pattern
   - Required parameters
   - Example
   - Any special notes
4. **Submit** a pull request

**Testing Requirements:**
- Verify against live Blogger instances
- Test both blogspot.com and custom domains
- Check mobile and desktop behavior
- Verify with authenticated and anonymous access

---

## 📄 License

This reference is provided under the **Creative Commons Attribution 4.0 International License**.

You are free to:
- **Share** — copy and redistribute the material in any medium or format
- **Adapt** — remix, transform, and build upon the material

Under these terms:
- **Attribution** — You must give appropriate credit, provide a link to the license, and indicate if changes were made.

**Disclaimer:** This is an unofficial reference. Blogger/Google may change endpoints, parameters, or behavior at any time. Use at your own risk.

---

## 🔗 Resources

- [Official Blogger API Documentation](https://developers.google.com/blogger/)
- [Blogger Help Center](https://support.google.com/blogger/)
- [Blogger Developers Forum](https://groups.google.com/g/bloggerdev)
- [Blogger Template Help](https://bloggertemplatehelp.blogspot.com/)

---

*Last Updated: November 2024*  
*Maintained by the Blogger community*  
*For the latest updates, star this repository on GitHub*

---

**Need machine-readable formats?** This reference is also available as:
- [JSON Schema](./schemas/blogger-urls.json)
- [Python Constants Module](./python/blogger_urls.py)
- [TypeScript Interfaces](./typescript/blogger.d.ts)
- [OpenAPI Specification](./openapi/blogger-api.yml)
- [Postman Collection](./postman/Blogger-API.postman_collection.json)

Check the `formats/` directory or request generation via Issues.
