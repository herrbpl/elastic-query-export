# elastic-query-export

Export Data from ElasticSearch to CSV by Raw or Lucene Query (e.g. from Kibana).
Works with ElasticSearch 6+ (OpenSearch works too) and makes use of ElasticSearch's Scroll API and Go's
concurrency possibilities to work as fast as possible.

## Install

Download a pre-compiled binary for your operating system from here: https://github.com/pteich/elastic-query-export/releases
You need just this binary. It works on OSX (Darwin), Linux and Windows.

## Usage

````bash
es-query-export -c "http://localhost:9200" -i "logstash-*" --start="2019-04-04T12:15:00" --fields="RemoteHost,RequestTime,Timestamp,RequestUri,RequestProtocol,Agent" -q "RequestUri:*export*"
````

## CLI Options

| Flag             | Default               |                                                                                                         | 
|------------------|-----------------------|---------------------------------------------------------------------------------------------------------|
| `-h --help`      |                       | show help                                                                                               |
| `-v --version`   |                       | show version                                                                                            |
| `-c --connect`   | http://localhost:9200 | URI to ElasticSearch instance                                                                           | 
| `-i --index`     | logs-*                | name of index to use, use globbing characters * to match multiple                                       |
| `-q --query`     |                       | Lucene query to match documents (same as in Kibana)                                                     |
| `   --fields`    |                       | define a comma separated list of fields to export                                                       |
| `-o --outfile`   | output.csv            | name of output file                                                                                     |
| `-f --outformat` | csv                   | format of the output data: possible values csv, json, raw                                               |
| `-r --rawquery`  |                       | optional raw ElasticSearch query JSON string                                                            |
| `-s --start`     |                       | optional start date - Format: YYYY-MM-DDThh:mm:ss.SSSZ. or any other Elasticsearch default format       |
| `-e --end`       |                       | optional end date - Format: YYYY-MM-DDThh:mm:ss.SSSZ. or any other Elasticsearch default format         |
| `--timefield`    |                       | optional time field to use, default to @timestamp                                                       |
| `--verifySSL`    | true                  | optional define how to handle SSL certificates                                                          |
| `--user`         |                       | optional username                                                                                       |
| `--pass`         |                       | optional password                                                                                       |
| `--size`         | 1000                  | size of the scroll window, the more the faster the export works but it adds more pressure on your nodes |
| `--trace`        | false                 | enable trace mode to debug queries send to ElasticSearch                                                |

## Output Formats

- `csv` - all or selected fields separated by comma (,) with field names in the first line 
- `json` - all or selected fields as JSON objects, one per line
- `raw` - JSON dump of matching documents including id, index and _source field containing the document data. One document as JSON object per line.
