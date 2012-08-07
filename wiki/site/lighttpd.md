# Hosting With Lighttpd

Markdoc does not add the .html suffix when it creates internal links, so
some minor rewriting is required.

    $HTTP["host"] =~ "www.mcdevs.org" {
        url.redirect = ( "^/(.*)" => "http://mcdevs.org/$1" )
    }

    $HTTP["host"] == "mcdevs.org" {
        server.name = "mcdevs.org"
        server.document-root = "/var/www/mcdevs.org/"

        url.rewrite-once = ("^(.*)/$" => "$1/")
        url.rewrite-if-not-file = (
            "^(.*)$" => "$1.html"
        )
    }

## Updating The Site

The site is updated using nothing more than rysnc. For example:

    markdoc build
    rsync -az .html/ tyler@tkte.ch:/var/www/mcdevs.org/

## Running Locally

    markdoc build
    markdoc serve