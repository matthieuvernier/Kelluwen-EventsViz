How to load sample-news-events.json into ElasticSearch :

1/ Create the index into ElasticSearch with the right configuration and fields mapping

curl -XPOST localhost:9200/news -d '
{
"settings" : {
"number_of_shards" : 5 ,
"number_of_replicas" : 1
},
"mappings" : {
"events" : {
"_source" : { "enabled" : false },
"properties" : {
"startDate" : { "type" : "date", "format" : "dd-MM-YYYY"},
"endDate" : { "type" : "date", "format" : "dd-MM-YYYY"},
"category" : { "type" : "string", "index" : "not_analyzed" },
"locations" : {
"properties" : {
"name" : {"type" : "string", "index" :"not_analyzed"},
"typeLoc" : {"type" : "string", "index" :"not_analyzed"},
"location" : {"type" : "geo_point"}
}
},
"keywords" : {
"properties" : {
"lemma" : {"type" : "string", "index" :"not_analyzed"},
"relevancy" : {"type" : "double"}
}
},
"articles_id" : {"type" : "string", "index" : "not_analyzed"}
}
}
}
}'

2/ Load the json file

curl -XPOST 'localhost:9200/news/_bulk?pretty' --data-binary @sample-news-events.json



