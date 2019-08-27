# REST API 2.0 Discogs.com client

[![Build Status](https://travis-ci.org/irlndts/go-discogs.svg?branch=master)](https://travis-ci.org/irlndts/go-discogs)[![Go Report Card](https://goreportcard.com/badge/github.com/irlndts/go-discogs)](https://goreportcard.com/report/github.com/irlndts/go-discogs)

go-discogs is a Go client library for the [Discogs API](https://www.discogs.com/developers/). Check the usage section to see how to access the Discogs API.

### Feauteres
 * Database
  * [Releases](#releases)
  * Release Rating
  * Master Releases
  * Master Versions
  * Artists
  * Artist Releases
  * Label
  * All Label Releases
 * [Search](#search)
 
Install
--------
    go get -u github.com/irlndts/go-discogs

Usage
---------
The discogs package provides a client for accessing the Discogs API. 
First of all import library and init client variable. According to discogs api documentation you [must provide your user-agent](https://www.discogs.com/developers/#page:home,header:home-general-information). 
```go
import "github.com/irlndts/go-discogs"
```

Some requests require authentification (as any user). According to [Discogs](https://www.discogs.com/developers/#page:authentication,header:authentication-discogs-auth-flow), to send requests with Discogs Auth, you have two options: sending your credentials in the query string with key and secret parameters or a [token parameter](https://www.discogs.com/settings/developers).
This is token way example:
```go
client, err := discogs.NewClient(&discogs.Options{
        UserAgent: "Some Name",
        Currency:  "EUR", // optional, "USD" (default), "GBP", "EUR", "CAD", "AUD", "JPY", "CHF", "MXN", "BRL", "NZD", "SEK", "ZAR" are allowed
        Token:     "Some Token", // optional
    })
``` 

#### Releases
```go
  release, _ := client.Database.Release(9893847)
  fmt.Println(release.Artists[0].Name, " - ", release.Title) 
  // St. Petersburg Ska-Jazz Review  -  Elephant Riddim
```

#### Search
Issue a search query to discogs database. This endpoint accepts pagination parameters.
Authentication (as any user) is required.

Use `SearchRequest` struct to create a request.
```go
type SearchRequest struct {
    Q             string // search query (optional)
    Type          string // one of release, master, artist, label (optional)
    Title         string // search by combined “Artist Name - Release Title” title field (optional)
    ReleaseTitle string // search release titles (optional)
    Credit        string // search release credits (optional)
    Artist        string // search artist names (optional)
    Anv           string // search artist ANV (optional)
    Label         string // search label names (optional)
    Genre         string // search genres (optional)
    Style         string // search styles (optional)
    Country       string // search release country (optional)
    Year          string // search release year (optional)
    Format        string // search formats (optional)
    Catno         string // search catalog number (optional)
    Barcode       string // search barcodes (optional)
    Track         string // search track titles (optional)
    Submitter     string // search submitter username (optional)
    Contributer   string // search contributor usernames (optional)

    Page     int // optional
    PerPage int // optional
}
```

Example
```go
  request := discogs.SearchRequest{Artist: "reggaenauts", ReleaseTitle: "river rock", Page: 0, PerPage: 1}
  search, _ := client.Search.Search(request)

  for _, r := range search.Results {
    fmt.Println(r.Title)
  }
```

etc. 
