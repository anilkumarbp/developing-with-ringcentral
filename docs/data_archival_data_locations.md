
# Data Locations

As the Data Records increase in volume, it plays a crucial role to archive the following:

* Call-Logs
  * Simple
  * Detailed
* Call Recordings
* Messages ( Message-Store )

### Call Logs

It becomes really important to archive the call-logs as the call volume grows. Consider a call log as shown below:
 
If you have already created call logs previously and you would like to use those calls to archive in this tutorial , just scope the request to the appropriate time range by adding the `dateTo` and `dateFrom` query parameters (remember, ISO 8601 formatted and URL Encoded strings).

```
GET /restapi/v1.0/account/~/extension/~/call-log?dateFrom=STARTING_DATE_TIME&dateTo=ENDING_DATE_TIME HTTP/1.1
Host: platform.devtest.ringcentral.com
Content-Type: application/json
Accept: application/json
Authorization: Bearer REPLACE_WITH_YOUR_VALID_ACCESS_TOKEN 
```

We should now have a list of call records, if you followed steps 1 through 3, you should see a list of all these records, but the legs for multi-legged calls is missing. We can see the three different recordings, but I made one of these calls to a call group from an outside line. 

```
{
    "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/extension/664573005/call-log?view=Simple&dateFrom=2016-06-05T23:11:00.000Z&page=1&perPage=100",
    "records": [
        {
            "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/extension/664573005/call-log/ASaxDDkSZ5s42MA?view=Simple",
            "id": "ASaxDDkSZ5s42MA",
            "sessionId": "13916417004",
            "startTime": "2016-06-06T23:07:20.000Z",
            "duration": 55,
            "type": "Voice",
            "direction": "Inbound",
            "action": "Phone Call",
            "result": "Accepted",
            "to": {
                "phoneNumber": "+15623215778",
                "name": "SDK Engineer Candidate"
            },
            "from": {
                "phoneNumber": "+14158905908",
                "name": "SAN FRANCSCO CA",
                "location": "San Francisco (South), CA"
            },
            "recording": {
                "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/recording/1662272004",
                "id": "1662272004",
                "type": "OnDemand",
                "contentUri": "https://media.devtest.ringcentral.com:443/restapi/v1.0/account/133128004/recording/1662272004/content"
            }
        },
        {
            "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/extension/664573005/call-log/ASaoSC9FIMaOF24?view=Simple",
            "id": "ASaoSC9FIMaOF24",
            "sessionId": "13914800004",
            "startTime": "2016-06-06T18:12:30.000Z",
            "duration": 55,
            "type": "Voice",
            "direction": "Outbound",
            "action": "VoIP Call",
            "result": "Call connected",
            "to": {
                "phoneNumber": "+14158905908",
                "location": "San Francisco (South), CA"
            },
            "from": {
                "phoneNumber": "+16505496100",
                "name": "SDK Engineer Candidate"
            },
            "recording": {
                "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/recording/1659910004",
                "id": "1659910004",
                "type": "OnDemand",
                "contentUri": "https://media.devtest.ringcentral.com:443/restapi/v1.0/account/133128004/recording/1659910004/content"
            }
        },
        {
            "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/extension/664573005/call-log/ASaoLzRiqjLaFYU?view=Simple",
            "id": "ASaoLzRiqjLaFYU",
            "sessionId": "13914782004",
            "startTime": "2016-06-06T18:07:41.000Z",
            "duration": 64,
            "type": "Voice",
            "direction": "Outbound",
            "action": "VoIP Call",
            "result": "Call connected",
            "to": {
                "phoneNumber": "+14158905908",
                "location": "San Francisco (South), CA"
            },
            "from": {
                "phoneNumber": "+16505496100",
                "name": "SDK Engineer Candidate"
            },
            "recording": {
                "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/recording/1659903004",
                "id": "1659903004",
                "type": "OnDemand",
                "contentUri": "https://media.devtest.ringcentral.com:443/restapi/v1.0/account/133128004/recording/1659903004/content"
            }
        }
    ],
    "paging": {
        "page": 1,
        "perPage": 100,
        "pageStart": 0,
        "pageEnd": 2
    },
    "navigation": {
        "firstPage": {
            "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/extension/664573005/call-log?view=Simple&dateFrom=2016-06-05T23:11:00.000Z&page=1&perPage=100"
        }
    }
}
```



5. Fetch the list of `Detailed` Call Logs so we can inspect the Detailed records. We do this by adding the `view` query paremter and setting it equal to `Detailed`.

```
GET /restapi/v1.0/account/~/extension/~/call-log?view=Detailed HTTP/1.1
Host: platform.devtest.ringcentral.com
Content-Type: application/json
Accept: application/json
Authorization: Bearer REPLACE_WITH_YOUR_VALID_ACCESS_TOKEN 
```

If everything worked, we should receive an HTTP 200 response with the JSON body which contain the `legs` property. Specifically, for the most recent item in the list which was used to make an inbound call to a Call Group, we can see the second leg being connected from the outside line to my Call Group main number:
(your numbers will differ, but the end result should be the similar if you have used a call group)


```
{
    "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/extension/664573005/call-log?view=Detailed&dateFrom=2016-06-05T23:09:00.000Z&page=1&perPage=100",
    "records": [
        {
            "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/extension/664573005/call-log/ASaxDDkSZ5s42MA?view=Detailed",
            "id": "ASaxDDkSZ5s42MA",
            "sessionId": "13916417004",
            "startTime": "2016-06-06T23:07:20.000Z",
            "duration": 55,
            "type": "Voice",
            "direction": "Inbound",
            "action": "Phone Call",
            "result": "Accepted",
            "to": {
                "phoneNumber": "+15623215778",
                "name": "SDK Engineer Candidate"
            },
            "from": {
                "phoneNumber": "+14158905908",
                "name": "SAN FRANCSCO CA",
                "location": "San Francisco (South), CA"
            },
            "recording": {
                "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/recording/1662272004",
                "id": "1662272004",
                "type": "OnDemand",
                "contentUri": "https://media.devtest.ringcentral.com:443/restapi/v1.0/account/133128004/recording/1662272004/content"
            },
            "transport": "PSTN",
            "lastModifiedTime": "2016-06-06T23:08:23.336Z",
            "legs": [
                {
                    "startTime": "2016-06-06T23:07:20.000Z",
                    "duration": 55,
                    "type": "Voice",
                    "direction": "Inbound",
                    "action": "Phone Call",
                    "result": "Accepted",
                    "to": {
                        "phoneNumber": "+15623215778",
                        "name": "SDK Engineer Candidate"
                    },
                    "from": {
                        "phoneNumber": "+14158905908",
                        "name": "SAN FRANCSCO CA",
                        "location": "San Francisco (South), CA"
                    },
                    "recording": {
                        "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/recording/1662272004",
                        "id": "1662272004",
                        "type": "OnDemand",
                        "contentUri": "https://media.devtest.ringcentral.com:443/restapi/v1.0/account/133128004/recording/1662272004/content"
                    },
                    "transport": "PSTN",
                    "legType": "Accept",
                    "extension": {
                        "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/extension/664573005",
                        "id": 664573005
                    }
                },
                {
                    "startTime": "2016-06-06T23:07:20.000Z",
                    "duration": 55,
                    "type": "Voice",
                    "direction": "Outbound",
                    "action": "VoIP Call",
                    "result": "Accepted",
                    "to": {
                        "phoneNumber": "+16506429233",
                        "location": "San Mateo, CA"
                    },
                    "from": {
                        "phoneNumber": "+14158905908",
                        "name": "SDK Engineer Candidate"
                    },
                    "recording": {
                        "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/recording/1662272004",
                        "id": "1662272004",
                        "type": "OnDemand",
                        "contentUri": "https://media.devtest.ringcentral.com:443/restapi/v1.0/account/133128004/recording/1662272004/content"
                    },
                    "transport": "VoIP",
                    "legType": "PstnToSip",
                    "extension": {
                        "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/extension/664573005",
                        "id": 664573005
                    }
                }
            ]
        },
        {
            "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/extension/664573005/call-log/ASaoSC9FIMaOF24?view=Detailed",
            "id": "ASaoSC9FIMaOF24",
            "sessionId": "13914800004",
            "startTime": "2016-06-06T18:12:30.000Z",
            "duration": 55,
            "type": "Voice",
            "direction": "Outbound",
            "action": "VoIP Call",
            "result": "Call connected",
            "to": {
                "phoneNumber": "+14158905908",
                "location": "San Francisco (South), CA"
            },
            "from": {
                "phoneNumber": "+16505496100",
                "name": "SDK Engineer Candidate"
            },
            "recording": {
                "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/recording/1659910004",
                "id": "1659910004",
                "type": "OnDemand",
                "contentUri": "https://media.devtest.ringcentral.com:443/restapi/v1.0/account/133128004/recording/1659910004/content"
            },
            "transport": "VoIP",
            "lastModifiedTime": "2016-06-06T18:13:43.335Z",
            "legs": [
                {
                    "startTime": "2016-06-06T18:12:30.000Z",
                    "duration": 55,
                    "type": "Voice",
                    "direction": "Outbound",
                    "action": "VoIP Call",
                    "result": "Call connected",
                    "to": {
                        "phoneNumber": "+14158905908",
                        "location": "San Francisco (South), CA"
                    },
                    "from": {
                        "phoneNumber": "+16505496100",
                        "name": "SDK Engineer Candidate"
                    },
                    "recording": {
                        "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/recording/1659910004",
                        "id": "1659910004",
                        "type": "OnDemand",
                        "contentUri": "https://media.devtest.ringcentral.com:443/restapi/v1.0/account/133128004/recording/1659910004/content"
                    },
                    "transport": "VoIP",
                    "legType": "SipToPstnUnmetered",
                    "extension": {
                        "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/extension/664573005",
                        "id": 664573005
                    }
                }
            ]
        },
        {
            "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/extension/664573005/call-log/ASaoLzRiqjLaFYU?view=Detailed",
            "id": "ASaoLzRiqjLaFYU",
            "sessionId": "13914782004",
            "startTime": "2016-06-06T18:07:41.000Z",
            "duration": 64,
            "type": "Voice",
            "direction": "Outbound",
            "action": "VoIP Call",
            "result": "Call connected",
            "to": {
                "phoneNumber": "+14158905908",
                "location": "San Francisco (South), CA"
            },
            "from": {
                "phoneNumber": "+16505496100",
                "name": "SDK Engineer Candidate"
            },
            "recording": {
                "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/recording/1659903004",
                "id": "1659903004",
                "type": "OnDemand",
                "contentUri": "https://media.devtest.ringcentral.com:443/restapi/v1.0/account/133128004/recording/1659903004/content"
            },
            "transport": "VoIP",
            "lastModifiedTime": "2016-06-06T18:09:03.334Z",
            "legs": [
                {
                    "startTime": "2016-06-06T18:07:41.000Z",
                    "duration": 64,
                    "type": "Voice",
                    "direction": "Outbound",
                    "action": "VoIP Call",
                    "result": "Call connected",
                    "to": {
                        "phoneNumber": "+14158905908",
                        "location": "San Francisco (South), CA"
                    },
                    "from": {
                        "phoneNumber": "+16505496100",
                        "name": "SDK Engineer Candidate"
                    },
                    "recording": {
                        "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/recording/1659903004",
                        "id": "1659903004",
                        "type": "OnDemand",
                        "contentUri": "https://media.devtest.ringcentral.com:443/restapi/v1.0/account/133128004/recording/1659903004/content"
                    },
                    "transport": "VoIP",
                    "legType": "SipToPstnUnmetered",
                    "extension": {
                        "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/extension/664573005",
                        "id": 664573005
                    }
                }
            ]
        }
    ],
    "paging": {
        "page": 1,
        "perPage": 100,
        "pageStart": 0,
        "pageEnd": 2
    },
    "navigation": {
        "firstPage": {
            "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/extension/664573005/call-log?view=Detailed&dateFrom=2016-06-05T23:09:00.000Z&page=1&perPage=100"
        }
    }
}
```

6. Inspecting the `legs` property of the multi-leg call, we can see two (2) different `legType` properties, the most recent (legs[0]) was the `Accept` which occurred when I accepted the call in my SoftPhone, while the former (legs[1]) was the `PstnToSip` which occurred when the inbound call from my celluar device was pointed by RingCentral to the IVR setup within my account. The `from.name` property would not make sense if you were to neglect looking at the call legType.


"legs": [
    {
        "startTime": "2016-06-06T23:07:20.000Z",
        "duration": 55,
        "type": "Voice",
        "direction": "Inbound",
        "action": "Phone Call",
        "result": "Accepted",
        "to": {
            "phoneNumber": "+15623215778", // The direct number of the Call Group defined in RingCentral
            "name": "SDK Engineer Candidate" // Name of the Extension that handled the call
        },
        "from": {
            "phoneNumber": "+14158905908",
            "name": "SAN FRANCSCO CA",
            "location": "San Francisco (South), CA"
        },
        "recording": {
            "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/recording/1662272004",
            "id": "1662272004",
            "type": "OnDemand",
            "contentUri": "https://media.devtest.ringcentral.com:443/restapi/v1.0/account/133128004/recording/1662272004/content"
        },
        "transport": "PSTN",
        "legType": "Accept",
        "extension": {
            "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/extension/664573005",
            "id": 664573005
        }
    },
    {
        "startTime": "2016-06-06T23:07:20.000Z",
        "duration": 55,
        "type": "Voice",
        "direction": "Outbound",
        "action": "VoIP Call",
        "result": "Accepted",
        "to": {
            "phoneNumber": "+16506429233",
            "location": "San Mateo, CA"
        },
        "from": {
            "phoneNumber": "+14158905908",
            "name": "SDK Engineer Candidate"
        },
        "recording": {
            "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/recording/1662272004",
            "id": "1662272004",
            "type": "OnDemand",
            "contentUri": "https://media.devtest.ringcentral.com:443/restapi/v1.0/account/133128004/recording/1662272004/content"
        },
        "transport": "VoIP",
        "legType": "PstnToSip",
        "extension": {
            "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/133128004/extension/664573005",
            "id": 664573005
        }
    }
] 


### Call Recordings

* How to Obtain Call Recording Metadata and Content

If you have followed all the steps above, you should now have at least one (more if you have done this before) call with a recording listed in the [Sandbox Online Account Portal - Call Log View](https://service.devtest.ringcentral.com/settings/calls.html#simple). If you do not see this record in the Sandbox Online Account Portal, wait a few minutes and check again...occasionally there is some latency.

1. Obtain a new RingCentral access_token using the credentials for the user you logged into the RingCentral Soft Phone with to create the recording.

```
POST /restapi/oauth/token HTTP/1.1
Host: platform.devtest.ringcentral.com
Authorization: Basic YOUR_BASE_64_ENCODED_API_KEYS_SEPARATED_BY_A_COLON
Accept: application/json
Content-Type: application/x-www-form-urlencoded
Cache-Control: no-cache

grant_type=password&username=RC_USERNAME_WITHOUT_PLUS_SYMBOL&password=YOUR_RC_PASSWORD_URL_ENCODED&extension=YOUR_RC_EXTENSION
```

2. Fetch the list of Call Logs for the currently authenticated user

```
GET /restapi/v1.0/account/~/extension/~/call-log HTTP/1.1
Host: platform.devtest.ringcentral.com
Content-Type: application/json
Accept: application/json
Authorization: Bearer YOUR_VALID_ACCESS_TOKEN
Cache-Control: no-cache
```

This should return a list of call logs.

3. The Call Recording Metadata is actually contained within the `recordings` property of the response body JSON. It should have a format similar to the following:

```
"recording": {
    "uri": "https://platform.devtest.ringcentral.com/restapi/v1.0/account/ACCOUNT_ID/recording/RECORDING_ID",
    "id": "RECORDING_ID",
    "type": "OnDemand",
    "contentUri": "https://media.devtest.ringcentral.com:443/restapi/v1.0/account/ACCOUNT_ID/recording/RECORDING_ID/content"
}
```

Of course, you can use the `recordings[item].uri` property to obtain the Call Recording Metadata as well, but that is redundant.

4. You can now use the `contentUri` (notice that the subdomain is different in some cases as `media...` instead of `platform...`) to obtain the Call Recording Content.

5. If you would like, you can use HTML5 supported browsers and the `<audio>` tag in a web page, and set the `src` property of the `<audio>` tag to the `recordings[item].contentUri` value (make sure to include the access_token query parameter with a valid access_token), and play back the recording in the browser.


### Messages ( in Message Store )

