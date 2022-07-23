# amq_streams_camelk_integration

## Prerequisites

- OpenShift CLI (oc) is installed
  <br/> (see here https://docs.openshift.com/container-platform/4.8/cli_reference/openshift_cli/getting-started-cli.html)
- Camel-K CLI (kamel) is installed
  <br/> (see here https://access.redhat.com/documentation/en-us/red_hat_integration/2020-q4/html-single/deploying_camel_k_integrations_on_openshift/index#installing-camel-k-client-tool)

The following Camel-K integration required a Kafka cluster to be available. 
Check the following repository to see how to create one on OpenShift:
https://github.com/mkossatz/messaging_with_ocp_amq_streams


## Install Camel-K Operator

1. Define OpenShift namespace in ops/camelk-operator.yaml
2. Apply the Camel-K Operator configuration 
<br/> (make sure you are in the correct namespace)

    ```Shell
    oc apply -f ops/camelk-operator.yaml
    ```

3. Review the pods state to see when the operator is running

    ```Shell
    oc get pods -w
    ```

## Deploy the Integration

1. Update any configurations necessary in the applications.properties file (should work by default)
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

5. Inspect the logs of the integration to see incoming messages

    ```Shell
    kamel log kafka-consumer
    ```