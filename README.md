# Webchat Proxy - Reference Implementation
Note: We encourage teams to decide their chosen Webchat based on user needs. This is reference implementation example for HMRC's eGain Webchat and should not be used in production.


## Motivation
This Webchat proxy is a reference implementation example of how to normalise a Webchat API to the GOV.UK Webchat Component needs, and this particular implementation is for eGain.
This API will take HMRC's eGain endpoints, and normalise them so that they're generic.

Departments within government have multiple Webchat integrations, the javascript API needs a unified way of
handling these integrations.

This proxy will give departments flexibility over implementing caching or changing internals of their endpoint, if it is needed without GOV.UK's support.

## Webchat Indentifier Mappings
Currently we keep a hardcoded mapping of URLs to their respective Webchat APIs. In the future we plan to allow these to be set in the Publishing Admin interface, but currently these will need to be handled in collaboration with GOV.UK.

## API Description
Anything more than a 200 response is a failure mode, JSON is returned, and CORS headers are set so this can be accessed by a browser.

---
### GET `/availability/:id`

`:id` should be the unique Webchat 'room' identifier

This endpoint is for checking the availability of a Webchat instance.

#### Responses

##### Webchat has a technical error
Any response that does not conform to an available response type will render a technical error,
as will a non 200 type response.

```
  {
    status: "failure",
    response: "error-message-related-to-the-techincal-error"
  }

```
##### Webchat is available
```
  response code: 200
  {
    status: "success",
    response: "AVAILABLE"
  }
```
##### WebChat is unavailable
```
  response code: 200
  {
    status: "success",
    response: "UNAVAILABLE"
  }
```
##### WebChat is busy
```
  response code: 200
  {
    status: "success",
    response: "BUSY"
  }
```
---

### GET `/open/:id`

`:id` should be the unique Webchat room identifier

This URL is used by the Webchat Component to open the correct Webchat instance. The Webchat component should be able to call `window.open` on this endpoint and open a Webchat instance in a new window.

In our reference example, we redirect to the underlying eGain API endpoint but this would change depending on your Webchat provider.

## This projects' dependencies

 - ruby (2.1>)
 - bundler

## How to run this project
```
git clone git@github.com:alphagov/reference-webchat-proxy.git
cd reference-webchat-proxy
bundle install
ruby app.rb
```
### Further documentation

The javascript and html side of this api is documented here.

https://github.com/alphagov/government-frontend/blob/master/docs/webchat.md
