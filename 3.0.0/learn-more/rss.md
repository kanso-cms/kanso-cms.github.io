# RSS

--------------------------------------------------------

Kanso has built in RSS support for feed readers to access a site and read their respective feed formats.

- [Feed Types](#feed-types)
- [Feed URLS](#feed-urls)

--------------------------------------------------------

### Feed types

By default, Kanso supports feeds in the following formats:

- RSS 2.0
- ATOM
- RDF

--------------------------------------------------------

### Feed URLS

Kanso routes feeds for the following pages and URL structures. With each feed url an additional (optional) format slug can be added to the end of the url to output the respective feed format. The slugs are `rss`, `rdf`, `atom`

#### Homepage
`http://example.com/feed/`  
`http://example.com/feed/rss/`  
`http://example.com/feed/atom/`  
`http://example.com/feed/rdf/`

#### Post
`http://example.com/permalink/feed/`  
`http://example.com/permalink/feed/rss/`  
`http://example.com/permalink/feed/atom/`  
`http://example.com/permalink/feed/rdf/`

#### Page
`http://example.com/slug/feed/`  
`http://example.com/slug/feed/rss/`  
`http://example.com/slug/feed/atom/`  
`http://example.com/slug/feed/rdf/`

#### Author taxonomy
`http://example.com/authors/slug/feed/`  
`http://example.com/authors/slug/feed/rss/`  
`http://example.com/authors/slug/feed/atom/`  
`http://example.com/authors/slug/feed/rdf/`

#### Tag taxonomy
`http://example.com/tag/slug/feed/`  
`http://example.com/tag/slug/feed/rss/`  
`http://example.com/tag/slug/feed/atom/`  
`http://example.com/tag/slug/feed/rdf/`

#### Category taxonomy
`http://example.com/category/slug/feed/`  
`http://example.com/category/slug/feed/rss/`  
`http://example.com/category/slug/feed/atom/`  
`http://example.com/category/slug/feed/rdf/`