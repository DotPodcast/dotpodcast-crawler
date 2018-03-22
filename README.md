## DotPodcast Blockstack Crawler

The DotPodcast Blockstack Crawler is a set of decoupled services that:

1. Page through names registered in the `.podcast` namespace in
Blockstack.
1. For each name, get the DotPodcast-compliant zonefile associated with that name.
1. Use the URI in the zonefile to crawl through the podcast and episode
JSON files.
1. Index those file into an Elasticsearch cluster.

The reasoning for adding a more traditional, Web 2.0 system in the mix
was to improve the search experience. Without crawling an indexing in a
separate service, this would have to be done in the user's browser,
which takes a lot of time and likely exceeds local storage limits.

Each numbered step above corresponds to a separate project:

1. [crawler-namespace-to-rabbit](https://github.com/DotPodcast/crawler-namespace-to-rabbit)
1. [crawler-zonefile-lookup](https://github.com/DotPodcast/crawler-zonefile-lookup)
1. [crawler-zonefile-uri-crawler](https://github.com/DotPodcast/crawler-zonefile-uri-crawler)
1. [crawler-elasticsearch-persistence](https://github.com/DotPodcast/crawler-elasticsearch-persistence)

![Crawler Diagram](/crawler-diagram.png)

### Running the crawler locally

In order to run the crawler locally, you'll need to be able to point to
a RabbitMQ instance and an Elasticsearch instance. The location of these
instances are configurable in each project via a root-level
`config.json` file and the default is to look on their default ports
exposed on `localhost`. To get up and running immediately, use the
provided `docker-compose.yml`:

```
docker-compose up
```

You should be able to get a response from Elasticsearch at
`http://localhost:9200/`. The RabbitMQ management console should be
available at `http://localhost:15672/`. The default username and
password is `guest`/`guest`. Here you can monitor queues.

Then, each service needs to be spun up separately. Each of this is a
Node.js project, and can be run using `npm` or `yarn` (we use `yarn`).
You can find specific details on configuration and running these
services in their respective README files.
