Basic Statistics of Common Crawl Monthly Archives
=================================================

Analyze the [Common Crawl](http://commoncrawl.org/) to get some crawl statistics:
* size of the monthly crawls
  * fetched pages
  * unique URLs
  * unique documents (by content digets)
  * number of different hosts, domains, top-level domains
* distribution of pages/urls on hosts, domains, tlds
* and ...
  * mime types
  * protocols / schemes (http vs. https)


Step 1: Count Items
-------------------

The items (URLs, hosts, domains, etc.) are counted using the Common Crawl index files
on AWS S3 `s3://commoncrawl/cc-index/collections/*/indexes/cdx-*.gz`.

1. create a list of cdx files to process - usually from one monthly crawl (here: `CC-MAIN-2016-26`)
   but could be multiple crawls in one turn or also a subset for testing:
   ```
   for i in `seq 0 299`; do
        echo s3://commoncrawl/cc-index/collections/CC-MAIN-2016-26/indexes/cdx-`printf %05d $i`.gz
   done >input-2016-26.txt
   ``` 

2. run `crawlstats.py --job=count` to process the cdx files and count the items:
   ```
   python3 crawlstats.py --job=count --logging-level=info --no-exact-counts --no-output --output-dir .../ .../input-2016-26.txt
   ```

Step 2: Analyze Counts
----------------------

Run `crawlstats.py --job=stats` on the output of step 1.