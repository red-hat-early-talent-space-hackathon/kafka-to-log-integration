# kafka-to-log-integration-example

## Prerequisites

- OpenShift CLI (oc) is installed locally
- Camel-K CLI (kamel) is installed locally

## Deploy the Integration

1. Update any configurations necessary in the applications.properties file (probably just the topics you want to consume from)
2. Create a ConfigMap on OpenShift, which contains the integrations configuration

    ```Shell
    oc create configmap kafka-to-log-integration.props  --from-file=application.properties
    ```

3. Deploy the integration

    ```Shell
    kamel run KafkaToLogIntegration.java --config configmap:kafka-to-log-integration.props
    ```

4. Check the state of the deployment

    ```Shell
    kamel get
    ```

    Note: The CamelK operator might take some time to build the integration. Wait for it to no longer say "Building Kit" - it needs to say "Running".

5. Inspect the logs of the integration to see incoming messages

    ```Shell
    kamel log kafka-to-log-integration
    ```