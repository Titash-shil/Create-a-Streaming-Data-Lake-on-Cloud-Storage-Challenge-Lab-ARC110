# Create a Streaming Data Lake on Cloud Storage: Challenge Lab || [ARC110](https://www.cloudskillsboost.google/games/5630/labs/36115) ||

# # Like, comment, share & Don't forget to subscribe [Qwiklab_Explorers_ts](https://youtube.com/@titashshil?si=RgamNu1dc9jVIbJN) 👍😄🤝

### Run the following Commands in CloudShell

```
export topic=
export message=""
export region=
```
```

gcloud auth list

gcloud config set compute/region $REGION

gcloud services disable dataflow.googleapis.com
gcloud services enable \
dataflow.googleapis.com \
cloudscheduler.googleapis.com


sleep 45


PROJECT_ID=$(gcloud config get-value project)
BUCKET_NAME="${PROJECT_ID}-bucket"


gsutil mb gs://$BUCKET_NAME

gcloud pubsub topics create $TOPIC_ID


# Set the App Engine region variable
if [ "$REGION" == "us-central1" ]; then
  gcloud app create --region us-central
elif [ "$REGION" == "europe-west1" ]; then
  gcloud app create --region europe-west
else
  gcloud app create --region "$REGION"
fi
 

gcloud scheduler jobs create pubsub publisher-job --schedule="* * * * *" \
    --topic=$TOPIC_ID --message-body="$MESSAGE"



#!/bin/bash

while true; do
    if gcloud scheduler jobs run publisher-job --location="$REGION"; then
        echo "Command executed successfully. Now running next command.."
        break 
    else
        echo "Retrying.. Please like and subscribe to QWIKLAB_EXPLORERS_TS[https://www.youtube.com/@qwiklabexplorers]."
        sleep 10 
    fi
done




cat > QWIKLAB_EXPLORERS.sh <<EOF_CP
#!/bin/bash

git clone https://github.com/GoogleCloudPlatform/python-docs-samples.git
cd python-docs-samples/pubsub/streaming-analytics
pip install -U -r requirements.txt
python PubSubToGCS.py \
--project=$PROJECT_ID \
--region=$REGION \
--input_topic=projects/$PROJECT_ID/topics/$TOPIC_ID \
--output_path=gs://$BUCKET_NAME/samples/output \
--runner=DataflowRunner \
--window_size=2 \
--num_shards=2 \
--temp_location=gs://$BUCKET_NAME/temp
EOF_CP

chmod +x QWIKLAB_EXPLORERS.sh


docker run -it -e DEVSHELL_PROJECT_ID=$DEVSHELL_PROJECT_ID -e BUCKET_NAME=$BUCKET_NAME -e PROJECT_ID=$PROJECT_ID -e REGION=$REGION -e TOPIC_ID=$TOPIC_ID -v $(pwd)/techcps.sh:/techcps.sh python:3.7 /bin/bash -c "/QWIKLAB_EXPLORERS.sh"
```

# Congratulations ..!!🎉  You completed the lab shortly..😃💯

# *Well done..!* 👏

# Thank you for visiting.... :) 🗯️

# [Qwiklab_Explorers_ts](https://youtube.com/@titashshil?si=RgamNu1dc9jVIbJN)
