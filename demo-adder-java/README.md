#demo-adder-java

A Java-based sample analytic for the Predix Analytics platform.

## Compiled binaries
Refer to the [Releases](https://github.com/PredixDev/predix-analytics-sample/releases) page for compiled binaries you can upload directly to Predix Analytics.

## Pre-requisites
To build and run this analytic, you will need to have the following:

- JDK 1.7+
- Maven 3+

## Building, deploying and running the analytic
1. From this directory, run the `mvn clean package` command to build and perform the component test, or just get the latest demo-adder-java-1.0.0.jar binary from the [Releases](https://github.com/PredixDev/predix-analytics-sample/releases) page.
2. Create an analytic in the Analytics Catalog with the name "Demo Adder Java" and the version "v1".
3. Upload the jar file demo-adder-java-1.0.0.jar from the demo-adder-java/target directory and attach it to the created analytic entry.
4. Deploy and test the analytic on Predix Analytics platform.

## Analytic template
This analytic takes in 2 numbers and returns their sum. This structure is outlined in this [analytic template](../demo-adder-template.json).

## Input format
The expected JSON input data format is as follows:
`{"number1": 123, "number2": 456}`

## Output format
The JSON output format from the analytic is as follows:
`{"result":579}`

## Developing a java-based analytic
1. Implement the analytic (and test functions) according to your development guidelines.
2. Create an entry-point method.  The entry method signature must be in one of the following two formats:
 * For analytics that do not use trained models, use the following signature for your entry method:
  `public String entry_method(String inputJson)`
 * For analytics that use trained models, use the following signature for your entry method:
  `public String entry_method(String inputJson, Map<String, byte[]> inputModels)`
 * In either case, the `entry_method` can be any method name. `inputJson` is the JSON string input that will be passed to the analytic. The output of this method must also be a JSON string.
 * `inputModels` contains a map of trained models as defined in the port-to-field map. The entry method should properly handle the case of an empty map.
3. Create the JSON configuration file `src/main/resources/config.json` containing the className and MethodName definitions that instruct the generated wrapper code to call your designated entry point method with the request payload.
4. Build and prepare the analytic jar file including `config.json` file and dependent jar files. See [sample pom.xml](pom.xml) for reference.

In this example, the entry-point is `add2Numbers` in the [DemoAdderJavaEntryPoint](src/main/java/com/ge/predix/analytics/demo/java/DemoAdderJavaEntryPoint.java) class.
[config.json](src/main/resources/config.json) properly maps the entry point to the `add2Numbers` method of the `DemoAdderJavaEntryPoint` class. 
This method takes in a JSON String, maps it to a HashMap (see line 19), performs the computation, and returns the result as a new JSON String (see line 27).

## Deploying the analytic to the Predix Cloud
When you upload the jar file as an 'Executable' artifact the platform wraps the executable as a web service exposing the analytic via a URI derived from the analytic name. 
Requests made to this generated URI will be passed to the entry point method.



For more information, see [Java Analytic Development](https://www.predix.io/docs#S1Wl9PHG) in the Predix Analytics Services documentation on Predix IO.