Doorman
-------
This will greet your coworkers on slack when they enter the office.

This was send in as part of the [AWS Deeplens Hackaton](https://devpost.com/software/doorman-a1oh0e)


Setup
-----

- Create a bucket, remember the name, make sure that your deeplens user can write to this bucket.
- Create a Rekognition collection (and note the collecition id)
- Be sure to have the following env vars in your environment:
 o BUCKET_NAME=your-bucket-name
 o SLACK_API_TOKEN=your-slack-token
 o SLACK_CHANNEL_ID="slack-channel-id"
 o REKOGNITION_COLLECTION_ID="your-collection-id"

- Deploy the lambda functions with Serverles (eg: `sls deploy`), this will create a CF stack with your functions. Note the api gateway endpoint, you'll need it later

- Go into the deeplens console, and create a project, select the "Object detection" model
- Remove the `deeplens-object-detection` function, and add a function for `find_person`
- Deploy the application to your deeplens

- Go to the [slack api](https://api.slack.com/apps), and click "create a new app".
- Give a name, and select your workspace
- Activate:
 o Incoming webhooks
 o Interactive components (use the api gateway endpoint that you noted before, ignore `Load URL`)
 o Permissions: Install the app in your workspace, and note the token. You'll need `chat:write:bot`, `channels:read` and `incoming-webhook`.
- Deploy the app again with the new environment variables

That should be it. Whenever the Deeplens rekognizes someone, it will upload into the S3 bucket. Which will trigger the other lambda functions.
