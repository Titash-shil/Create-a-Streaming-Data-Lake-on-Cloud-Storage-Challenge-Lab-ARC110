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
gcloud config set compute/region $region
gcloud services disable dataflow.googleapis.com
gcloud services enable \
dataflow.googleapis.com \
cloudscheduler.googleapis.com
sleep 45
PROJECT_ID=$(gcloud config get-value project)
BUCKET_NAME="${PROJECT_ID}-bucket"
gsutil mb gs://$BUCKET_NAME
gcloud pubsub topics create $topic
if [ "$region" == "us-central1" ]; then
  gcloud app create --region us-central
elif [ "$region" == "europe-west1" ]; then
  gcloud app create --region europe-west
else
  gcloud app create --region "$region"
fi
gcloud scheduler jobs create pubsub publisher-job --schedule="* * * * *" \
    --topic=$topic --message-body="$message"
#!/bin/bash
while true; do
    if gcloud scheduler jobs run publisher-job --location="$region"; then
        echo "Command executed successfully. Now running next command.."
        break 
    else
        echo ""
        sleep 10 
    fi
done
cat > qwiklabs_explorers.sh <<EOF_CP
#!/bin/bash
git clone https://github.com/GoogleCloudPlatform/python-docs-samples.git
cd python-docs-samples/pubsub/streaming-analytics
pip install -U -r requirements.txt
python PubSubToGCS.py \
--project=$PROJECT_ID \
--region=$region \
--input_topic=projects/$PROJECT_ID/topics/$topic \
--output_path=gs://$BUCKET_NAME/samples/output \
--runner=DataflowRunner \
--window_size=2 \
--num_shards=2 \
--temp_location=gs://$BUCKET_NAME/temp
EOF_CP
chmod +x qwiklabs_explorers.sh
docker run -it -e DEVSHELL_PROJECT_ID=$DEVSHELL_PROJECT_ID -e BUCKET_NAME=$BUCKET_NAME -e PROJECT_ID=$PROJECT_ID -e region=$region -e topic=$topic -v $(pwd)/qwiklabs_explorers.sh:/qwiklabs_explorers.sh python:3.7 /bin/bash -c "/qwiklabs_explorers.sh"
```

- ### Go to `Dataflow` from [here](https://console.cloud.google.com/dataflow/jobs?referrer=search&cloudshell=true&project=qwiklabs-gcp-00-90085139e9a5)

# Congratulations ..!!🎉  You completed the lab shortly..😃💯

# *Well done..!* 👏

# Thank you for visiting.... :) 🗯️

# [Qwiklab_Explorers_ts](https://youtube.com/@titashshil?si=RgamNu1dc9jVIbJN)
