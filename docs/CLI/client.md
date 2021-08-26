# Assemblyline Client Library

The assemblyline client library facilitates issuing requests to assemblyline.

## Pre-requisites

To install the client you'll need to make sure the you have the folowing installed:

    # APT/YUM
    libffi-dev
    libssl-dev

    # pypi
    pycryptodome
    requests
    requests[security]
    python-baseconv
    python-socketio[client]
    socketio-client==0.5.7.4


## Using the client

### Creating an API Key
1. Click on your avatar in the top-right corner and select "Manage account"
2. Scroll down to the bottom of this page to the "Security" section and select "Manage API Keys"
3. Add the API Key name and select whether you want the user of this API Key to Read, Write or Read and Write to
this Assemblyline instance. Click "Add".
4. Copy it somewhere safe so that you can use it later.
5. Click "Done".

You can instantiate the client using the following snippet of code:

    # The new v4 client will test connection to detect if the server is v3 or v4. You should now use the get_client method.
    from assemblyline_client import get_client
    al_client = get_client("https://localhost:443", auth=('user', 'password'))

    # or with an apikey

    al_client = get_client("https://localhost:443", apikey=('user', 'key'))

    # or with a cert

    al_client = get_client("https://localhost:443", cert='/path/to/cert/file.pem')

    # and if your assemblyline server is using a self-signed cert

    al_client = get_client("https://localhost:443", auth=('user', 'password'), verify=False)
    al_client = get_client("https://localhost:443", auth=('user', 'password'), verify='/path/to/server.crt')

The assemblyline client is fully documented in the docstrings so if you use an interactive client like ipython you can use the help feature.

    al_client.search.alert?
    Signature: al_client.search.alert(query, *args, **kwargs)
    Docstring:
    Search alerts with a lucene query.

    Required:
    query   : Lucene query. (string)

    Search parameters can be passed as key/value tuples or keyword parameters.

    Returns all results.
    File:      /usr/local/lib/python2.7/dist-packages/assemblyline_client/__init__.py
    Type:      instancemethod

### Examples

#### Submit a file

There are two distinct api to send a file (or url), submit and ingest.

Both leverage the
[SubmissionParams class](https://github.com/CybercentreCanada/assemblyline-base/blob/fc5f8216e7fa59d9421ff626927d9602e5a3430c/assemblyline/odm/models/submission.py#L41). 
The main difference in terms of passing these parameters to submit and ingest is that the first is syncronous (blocking) and ingest is asyncronous(non-blocking, very fast). In addition ingest support alerting: ingest takes `alert` as a parameter in the method call.

Both leverage the following parameters (optional):

```python
settings = { 
    'classification' : 'TLP:A',     # classification
    'description' : 'Hello world',  # file description
    'name' : 'filename',            # file name
    'deep_scan' : True,             # activate deep scan mode
    'priority' : 100,               # queue priority (higher nb is higher priority)
    'ignore_cache' : 'false',       # ignore system cache
    'metadata' : {
        'my_metadata' : 'value'     # any metadata of your liking
    },
    'services' : {                  # selected service list (override user profile)
        'selected' : [
            'Cuckoo','Extract'
        ],
        'resubmit' : [],            # resubmit to these services if initial submission scores > 500
        'excluded': ['']            # exclude some services
    },
    'service_spec': {'Extract': {'password': 'password'}} # provide a service parameter (e.g password for extract service)
}
```
##### Submit

Submit accept file and return a submission id, this api is sync; which mean it is a blocking call (slower)
```python
al_client.submit('/path/to/my/file.txt', fname='filename', params=settings)
```
##### Ingest

Ingest accept a file and return an ingest id, you can provide a callback in the param. This call is async; which mean it is not blocking (very fast)

It also allow the system to generate alert if the score is 500 or higher and the alert parameter is set to True
```python
al_client.ingest('/path/to/my/file.txt', fname='filename', nq='notification_queue_name', alert=False, params=settings, metadata=metadata)
# If you use a notificaton queue you can get your results with:
al_client.ingest.get_message("notification_queue_name")
```

#### Getting a key

To get a key of a given bucket, you simply need to pass it it's ID

    submission_details = al_client.submission("4nxrpBePQDLH427aA8m3TZ")

#### Using search

You can use the search engine in the client by simply passing a lucene query

    search_res = al_client.search.submission("submission.submitter:user")

#### Using search iterator

Instead of using a strait search and getting a page of result, you can use the search iterator to go through all results.

    for submission in al_client.search.stream.submission("submission.submitter:user"):
        # It only return the indexed fields if you want the full thing you need to go get it
        full_submission = al_client.submission(submission['submission.sid'])

        # Then do stuff with full submission (print for example)
        print(full_submission)

#### Using search parameters

    c.search.facet.submission('submission.submitter', query='times.submitted:[NOW-7DAYS TO NOW]')

#### Listen for message instead of querying for data

You can listen on the different message queues and execute a callback on each message.

    def callback(callback_data):
        print callback_data

    al_client.socketio.listen_on_dashboard_messages(callback)

**NOTE**: Depending on the volume of data, you might process a ton of messages!

## Using a configuration file
Rather than passing authentication and server details as parameters in a command line, you can use a configuration file.
This configuration file should be placed at `~/.al/submit.cfg`. A template for this configuration 
file can be found below.
NOTE: You can use `=` or `:` as the delimiter between key and value.
```
[auth]
#   Username for Assemblyline account.
user = 
#   There are three methods to authenticate a user account. Choose one:
#   - Password Provided via User Prompt
#       Leave the `password' configuration value below empty. You will prompted to enter the password upon 
#       running 'al-submit'. You will have to enter this password every time that you use the 'al-submit' tool. 
#   - Password Provided in Configuration File
#       Enter the password for Assemblyline account in plaintext.
password = 
#   - API Key in Configuration File
#       Enter the API key to use in plaintext for the user to login.
#       NOTE: The API key must have WRITE access for INGEST and WRITE+READ for SUBMIT.
apikey = 
# Skip server cert validation. 
# Value can be one of: true, false, yes, no
# If not supplied, default value is: false
insecure = 
[server]
# Method of network transport. 
# If not supplied, default value is: https
transport = 
# Domain of Assemblyline instance.
# If not supplied, default value is: localhost
host = 
# Port to which traffic will be sent.
# If not supplied, default value is: 443
port = 
# Server cert used to connect to server.
cert = 
```

## Mass submitting files
When using Assemblyline to respond to a cybersecurity incident or just need to submit a lot of file, we recommend using the Ingest API.
There are [commandline interfaces](https://github.com/CybercentreCanada/assemblyline-incident-manager) that you can use to assist with this process.
There are also things to note prior to ingestion, and these are the [default ingest values](https://github.com/CybercentreCanada/assemblyline-base/blob/9d4ab5586ff34ae20e3a08e9584776379fc981e9/assemblyline/odm/models/config.py#L377
) of Assemblyline. 

It is important to look at the `"sampling_at"` values, as these are the queue size limits for ingestion. 
You have to keep your ingestion flow at a rate such that the size of the ingestion queue remains lower than the corresponding priority values, otherwise Assemblyline will skip files.