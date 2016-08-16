
## Common Use Case(s)

* Public Communications

    A public manufacturer needs to comply with the SOX requirements and retains all the electronic data. The company decides to use Dropbox Integration, which allowed them to automatically archive electronic communications, such as call recordings, voicemails, text messages, and faxes.
    The integration app aided the company in SOX compliance.

* Storing day-to-day Call Recordings

    A financial services company would be involved in storing the call-recordings for compliance.But would be limited with the storage capacity on premise.


## Development Concern(s)


* RingCentral Data Retention Policies

    RingCentral does not retain your [Call Recordings](https://developers.ringcentral.com/api-docs/latest/index.html#!#RefCallLogInfo.html) data indefinitely. This is one of the driving factors that cause developers to author integrations with RingCentral.

    The data retention policies for RingCentral accounts differ if your account is a HIPAA-Compliant, or Non-HIPAA-Compliant account. There are multiple ways you can download your call log data, please refer to the official [RingCentral KB Article on Message Storage and Data Retention](http://success.ringcentral.com/articles/en_US/RC_Knowledge_Article/2178#2) for comprehensive information about your account.

    ## Developer Implications

    Developers who are building monitoring and dashboard solutions which provide historical analysis features will need to pay close attention to how RingCentral Call Log Data Retention Policy.

    For instance, if your application or integration requires the ability to analyze Call Log data which goes beyond the current storage threshold, you will want to download your Call Log data at a regular interval into persistent storage for data-crunching and performance reasons.

    The below information about Data Retention is accurate as of (2016-06-06), please refer to the KB article referenced above for the most up-to-date information about RingCentral Data Retention policies.

    ### Account Data Retention Table for Non-HIPAA Accounts

    | Feature                                    | Duration                                             | Count -or- Size                                                                         |
    |--------------------------------------------|------------------------------------------------------|-----------------------------------------------------------------------------------------|
    | Automatic Call Recordings                  | 90 Days                                              | 100K Recordings per Account                                                             |
    | On-Demand Call Recordings                  | 90 Days                                              | 200 Recordings per Mailbox or User                                                      |
    | Fax / Voice Messages (Inbox, Outbox, Sent) | 30 Days                                              | 200 Messages per folder, per User (Inbox, Outbox, Sent) Fax and Voice messages combined |
    | Fax / Voice Messages (Deleted)             | 5 Days                                               | 200 Messages per User (Fax and Voice messages combined)                                 |
    | Text Messages                              | No Limit                                             | 5K Messages per folder, per User (Inbox, Outbox, Sent, Deleted)                         |
    | Call Log                                   | 12 months (9 months in mobile apps on some accounts) | No Limit                                                                                |

    ### Account Data Retention for HIPAA Accounts

    | Feature                                    | Duration                                             | Count -or- Size                                                                         |
    |--------------------------------------------|------------------------------------------------------|-----------------------------------------------------------------------------------------|
    | Automatic Call Recordings                  | 30 Days                                              | 100K Recordings per Account                                                             |
    | On-Demand Call Recordings                  | 30 Days                                              | 200 Recordings per Mailbox or User                                                      |
    | Fax / Voice Messages (Inbox, Outbox, Sent) | 30 Days                                              | 200 Messages per folder, per User (Inbox, Outbox, Sent) Fax and Voice messages combined |
    | Fax / Voice Messages (Deleted)             | 5 Days                                               | 200 Messages per User (Fax and Voice messages combined)                                 |
    | Text Messages                              | Not Available                                        | Not Available                                                                           |
    | Call Log                                   | 12 months (9 months in mobile apps on some accounts) | No Limit                                                                                |

    NOTE: Voice and Fax messages are not sent via email in HIPAA-Compliant accounts

    ## Dowloading Call Log Data via API

    The Call Log API resource is the perfect tool for downloading long-term data records into the persistent storage system of choice for developers.

    There are two levels of Call Log data available depending upon how you decide to build your integration in terms of the Role of the user who has authenticated:

    | Role of authenticated user | Call Log data available | Route                                             |
    |----------------------------|-------------------------|---------------------------------------------------|
    | Admin                      | Account-Level           | /restapi/v1.0/account/~/call-log                  |
    | Non-Admin (aka: Extension) | Extension-Level         | /restapi/v1.0/account/~/extension/~/call-log      |

    This is a very important distinction for developers which will impact application design decisions and heavily relate on the type of application you are building. Developers creating SaaS or cloud-based applications which use RingCentral Authorization Flow to obtain access_tokens, would need to modify the route used depending upon the scope of the view of data being represented. Compare this to developers who are building a Server-Only (No UI) application which uses Password Credentials (ROPC) to obtain an access_token for downloading the account-level call log data.

    // TODO: Add links to => How to Download Account-Level Call Log Data && Developing Call Log Dashboards


* Configuration Requirements

    * What is the hourly interval/ range when they would like to pull down the recordings
    * What is the approximate call-recordgins count per day
    * The total number of accounts and the details

* Needs-Driven Solution Architecture

    ## Retrieving a call recording as and when it is created:

    1.Create extension map: direct_number to extension_info by downloading all extensions with possible phone-number endpoint(?) and create direct_number lookup for the to.phoneNumber or from.phoneNumber.
    2.Call call-log API, for each calllog.record:
    3.Where direction=Inbound, classify using calllog.record.to.phoneNumber
    4.Where direction=Outbound, classify using calllog.record.from.phoneNumber
    5.Use x.phoneNumber to look up extension to get derive store_name for folder name
    6.Use calllog.record.startTime.YYYY-MM-DD as date_called folder name
    7.Filename = calllog.record.startTime + ‘_’ + recordingId + ‘.mp3’ or ‘.wav’ based Content-Type headerlrc

    ## Retrieving call recordings at the end of a business day:

    1.Retrieving the Call Logs specifying the `dateFrom` and `dateTo` be on a 24hr interval.
    2.Setting the appropriate offset with UTC, as the Call-Logs returned from API are in the UTC TimeStamp.
    3.Filtering the call-logs to contain only Call Recordings.
    4.Download the call recordings and make sure the API Rate limits are not throttled.

## Persistent Storage Solutions