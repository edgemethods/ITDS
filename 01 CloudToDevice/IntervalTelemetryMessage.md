# Interval Telemetry Message
## When
1.	On sampling interval determined by device settings
2.	On any on-demand event where telemetry is required
## Logical Contents
Telemetry that is measured by the device/sensor over a period and provided as a series of functions, such as maximum and average, over that period. 
## Format
* [MessageType](#messagetype) ```string```
* [Spec](#spec) ```string```
* [DeviceId](#deviceid) ```string```
* [MessageId](#messageid) ```Int32```
* Interval ```object```
    * [FromDateTime](#intervalfromdatetime) ```string```
    * [ToDateTime](#intervaltodatetime) ```string```
* * ComponentMeasurements ```object[]```
    * [ComponentCode](#componentmeasurementscomponentcode) ```string``` 
    * [Measurements](#componentmeasurementsmeasurements) ```object[]```
        * [Key](#componentmeasurementsmeasurementskey) ```string``` 
        * [UnitOfMeasure](#componentmeasurementsmeasurementsunitofmeasure) ```string``` 
        * [SampleCount](#componentmeasurementsmeasurementssamplecount) ```Int16```
        * [Functions](#componentmeasurementsmeasurementsfunctions) ```object[]```
            * [Function](#componentmeasurementsmeasurementsfunctionsfunction) ```string``` 
            * [Value](#componentmeasurementsmeasurementsfunctionsvalue) ```string``` 
* [NominalComponentMeasurements](#nominalcomponentmeasurements) ```object[]```
    * [ComponentCode](#nominalcomponentmeasurementscomponentcode) ```string``` 

### MessageType
```string``` = “IntervalTelemetryMessage”
### Spec
```string``` = “1.1.0.0”
### DeviceId
```string``` 
### MessageId
```Int32```
### Interval/FromDateTime
```string``` in DateTime Format ```yyyy-MM-ddTHH:mm:ssZ```
### Interval/ToDateTime
```string``` in DateTime Format ```yyyy-MM-ddTHH:mm:ssZ```
### ComponentMeasurements/ComponentCode
```string```
Component only needs to match component model if UnitOfMeasure is not populated
### ComponentMeasurements/Measurements
The list of measurements allows for measurement of different units for a particular sensor. For example, resistance (ohm) and the derived temperature (K). This is a useful aid in the diagnostics of sensor calibration.
### ComponentMeasurements/Measurements/Key
```string``` 
### ComponentMeasurements/Measurements/UnitOfMeasure
```string```

UnitOfMeasure is optional if component model contains UnitOfMeasure.
### ComponentMeasurements/Measurements/Functions
Functions are the statistical functions that have been run on the sample. Functions could include max, min, mean, sdev.

### ComponentMeasurements/Measurements/Functions/Function
```string```

### ComponentMeasurements/Measurements/Functions/Value
```string```

### NominalComponentMeasurements
If components are in nominal range, based on settings, component names are listed. For example, if the nominal operating range is 240V ± 10V, any values recorded in the range will not be detailed, and can be recorded as nominal. This is useful to reduce the amount of unnecessary data sent.
### NominalComponentMeasurements/ComponentCode
```string```

## Sample
```JSON

{
  "DeviceId": "OI000002",
  "Spec": "1.1.0.0",
  "MessageType": "IntervalTelemetryMessage",
  "MessageId": 0,
  "Interval": {
    "FromDateTime ": "2016-03-11T18:50:48Z",
    "ToDateTime ": "2016-03-11T18:51:48Z"
  },

  "NominalComponentMeasurements": ["P2", "V12"],
  "ComponentMeasurements": [
    {
      "ComponentCode": "P2",
      "SampleCount": "230",
      "Functions": [
        {
          "Function": "max",
          "Value": "0.512299"
        },
        {
          "Function": "min",
          "Value": "0.397775"
        },
        {
          "Function": "sdev",
          "Value": "10"
        }
      ]
    }
  ]
}
```
## Server-side validations
1.	Interval: Required
    1. [FromDateTime](#intervalfromdatetime): Required. Standard DateTime validation.
    2. [ToDateTime](#intervaltodatetime): Required. Standard DateTime validation.
    3. [FromDateTime](#intervalfromdatetime) must be less than ToDateTime. 
2.	ComponentMeasurements can only be empty if [NominalComponentMeasurements](#nominalcomponentmeasurements) contains data.
3.	If ComponentMeasurements exists:
    1. [ComponentCode](#componentmeasurementscomponentcode): Required.
    2. [Measurements](#componentmeasurementsmeasurements): Required. List cannot be empty.
        1. Key: Required.
        2. [Functions](#componentmeasurementsmeasurementsfunctions): Required. List cannot be empty.
            1.	[Function](#componentmeasurementsmeasurementsfunctionsfunction): Required.
            2.	[Value](#componentmeasurementsmeasurementsfunctionsvalue): Required.