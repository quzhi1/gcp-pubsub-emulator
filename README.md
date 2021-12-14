# GCP PubSub Emulator for Local Development

### Build the Docker image:
- `docker build . -t gcppubsub:latest`

### Run:
- `docker run -p 8085:8085 gcppubsub:latest`

### Test connection with curl:
- test with curl: `http://localhost:8085` or `0.0.0.0:8085`
- Host and ports can be passed as part of building the image

### Make REST API calls:
- Projects are created "on the fly" when creating topics
- Create a topic: `PUT http://localhost:8085/v1/projects/test/topics/cats` where `cats` is a new topic name and `test` is the project
- Get project topics `GET http://localhost:8085/v1/projects/test/topics`
- Subscribe to topic: `PUT http://localhost:8085/v1/projects/test/subscriptions/test-sub` where `test-sub` is the subscription name
   - Payload:
        ```
        {"topic": "projects/test/topics/cats"}
        ```

- Publish message to topic: `POST http://localhost:8085/v1/projects/test/topics/cats:publish`
    - Payload:
        ```
        ​{"messages": [{"data": "cats rule"}]}
        ````
- Pull messages from topic: `POST localhost:8085/v1/projects/test/subscriptions/test-sub:pull`
    - Payload:
        ```
        ​{"max_messages": 1}
        ````
- More: https://cloud.google.com/pubsub/docs/reference/rest
- Curl samples: https://github.com/robertsicoie/gcp-pubsub-samples

### Clean up build stage image:
- `docker image prune --filter label=stage=builder`

### Build multi-arch images
```bash
docker buildx build --platform linux/amd64,linux/arm64,linux/arm/v7 -t quzhi65222714/gcp-pubsub-emulator:latest --push .
```

### TODO:
- Do we need configurable hosts/ports?
