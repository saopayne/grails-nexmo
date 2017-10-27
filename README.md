# Grails-Nexmo Plugin

[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)

A Grails plugin to allow applications send sms, lookup a number and make calls. with text-to-speech using [Nexmo's API](https://www.nexmo.com/).

## Installation

##### Using Gradle

```compile 'com.saopayne:grails-nexmo:1.2'```

##### Using Maven
```
<dependency>
  <groupId>com.saopayne</groupId>
  <artifactId>grails-nexmo</artifactId>
  <version>1.2</version>
  <type>pom</type>
</dependency>
```

##### Using Ivy
```
<dependency org='com.saopayne' name='grails-nexmo' rev='1.2'>
  <artifact name='grails-nexmo' ext='pom' ></artifact>
</dependency>
```

##### Adding Jar File

https://dl.bintray.com/saopayne/plugins/com/saopayne/grails-nexmo/1.2/grails-nexmo-1.2.jar



## Methods


##### [__lookup(String apiKey, String apiSecret, String number)__](https://github.com/saopayne/grails-nexmo/blob/master/grails-app/services/NexmoService#L129)

This method allows you to perform a look up on a given number to get the carrier, cuntry code etc.

* Parameters
  * __apiKey__ - Your api key from the settings dashboard
  * __apiSecret__ - Your api secret from the settings dashboard
  * __number__  - The number to look up
* Returns
  * __carrier__ - The carrier details of the number such as the name and type
  * __country_code__ - The country code 

##### [__sendSms(String apiKey, String apiSecret, String callback, String to, String text, String from)__](https://github.com/saopayne/grails-nexmo/blob/master/grails-app/services/NexmoService#L129)

This method allows you to send an SMS message to a mobile number.

* Parameters
  * __apiKey__ - Your api key from the settings dashboard
  * __apiSecret__ - Your api secret from the settings dashboard
  * __to__ - The mobile number in international format
  * __text__ - Body of the text message (with a maximum length of 3200 characters)
  * __from__ (optional) - The number to send from
* Returns
  * __status__ - The status code of the message
  * __id__ - The ID of the message

#### [__call(String apiKey, String apiSecret, String to, String text, String from)__](https://github.com/saopayne/grails-nexmo/blob/master/grails-app/services/NexmoService#L153)

This method uses text-to-speech to call your recipient and deliver a message.

* Parameters
  * __apiKey__ - Your api key from the settings dashboard
  * __apiSecret__ - Your api secret from the settings dashboard
  * __to__ - The phone number to send the call to, in International Format
  * __text__ - The message to deliver during the call
  * __from__ (optional) - The number to send the call from. Must be a voice enabled inbound number associated with your account
* Returns
  * __status__ - The status code of the message
  * __id__ - The ID of the message

## Usage

You can proceed to use the plugin in a Grails controller or service by injecting the NexmoService bean, and calling the `sendSms` method.

### Example

```groovy
class SampleController {

  // Inject the service
  def nexmoService

  def index() {
    def lookUpResult
    def smsResult
    def callResult

    try {
      // Lookup a particular number
      lookUpResult = nexmoService.lookup("apiKey", "apiSecret", "070707") 
       
      // Send the message "hello Nexmo" to 0704303333
      smsResult = nexmoService.sendSms("apiKey","apiSecret", "", "0704303333", "Hello Nexmo")

      // Call the number and tell them a message
      callResult = nexmoService.call("020233323", "Have a great day! Goodbye.")
    } catch (NexmoException e) {
      // Handle error if failure
    }
  }
}
```

Here is an example response:

```groovy
// Lookup
[carrier:[name:carrierName, type:carrierType],country_code:countryCode]

// SMS
[ status: "0", id: "00000125" ]

// Call
[ status: "0", id: "14b75f2246e7c1a17d345449a20d93e5" ]
```

#### To contribute
- Fork this repository.
- Create a new branch and make additions as you deem fit.
- Send in a PR with what the PR does.
