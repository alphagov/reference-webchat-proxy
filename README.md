# webchat proxy
This webchat proxy is a reference implementation example of how to normalise a webchat api to what the GOV.UK webchat needs, this particular implementation is for egain. This api will take the egain response, and give us a response as to whether webchat is available

### motivation
Departments within government have multiple webchat integrations, the javascript api needs a unified way of
handling these integrations. To that end this project will normalise the responses so they are simple. It also
gives departments flexibility over implementing caching if it is needed.


### api description
Anything other than a 200 response is a failure mode, json is returned, and CORS headers are
set so this can be accessed by a browser.

---
##### GET `/availability` this endpoint is for getting the availability of the webchat implementation
###### The api sent back an unknown response

```
  response code: 500
  {
    status: "failure",
    response: "unknown"
  }

```
###### WebChat is available
```
  response code: 200
  {
    status: "success",
    response: AVAILABLE
  }
```
###### WebChat is unavailable
```
  response code: 200
  {
    status: "success",
    response: UNAVAILABLE
  }
```
###### WebChat is busy
```
  {
    status: "success",
    response: BUSY
  }
```
---

### dependencies

 - ruby (2.0>)
 - bundler
 - Enviroment variable (EGAIN_URL) this correpsonds to the egain endpoint for notifications

### to run
```
clone repo
cd into repo
bundle install
ruby app.rb
```
