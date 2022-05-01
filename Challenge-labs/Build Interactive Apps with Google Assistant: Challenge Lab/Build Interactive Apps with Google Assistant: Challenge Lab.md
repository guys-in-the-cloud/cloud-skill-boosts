# Build Interactive Apps with Google Assistant: Challenge Lab

[YouTube Video Link](https://youtu.be/ZdZ3SiarZrs)

## Let's start with defining some variables given by Cloud Skill Boosts

Defining Cloud Function name variable
```
export CLOUDFUNCTION_NAME=
```
Defining entrypoint variable
```
export ENTRYPOINT=
```

## Downloading main.py and requirements.txt file we will use this while creating cloud function 

```
wget https://github.com/guys-in-the-cloud/cloud-skill-boosts/raw/main/Challenge-labs/Build%20Interactive%20Apps%20with%20Google%20Assistant:%20Challenge%20Lab/main.py

wget https://github.com/guys-in-the-cloud/cloud-skill-boosts/raw/main/Challenge-labs/Build%20Interactive%20Apps%20with%20Google%20Assistant:%20Challenge%20Lab/requirements.txt

```

## Task 1: Create the Cloud Function for the Magic Eight Ball app

1.1 Replacing the variable with REPLACE_WITH_YOUR_ENTRY_POINT
```
sed -i "s/REPLACE_WITH_YOUR_ENTRY_POINT/$ENTRYPOINT/g" main.py
```

1.2 Deploy the function 
```
gcloud functions deploy $CLOUDFUNCTION_NAME \
    --entry-point $ENTRYPOINT \
    --runtime python39 \
    --max-instances 5 \
    --trigger-http --allow-unauthenticated \
    --region us-central1
```

## Task 2: Create the Magic 8 Ball app for Google Assistant
- Now in manual go to action console 
 ![image](https://user-images.githubusercontent.com/104570014/166137381-4165cba0-0477-471e-98e1-c469dea7188f.png)

- select new project --> yes and agree terms and condition
 ![image](https://user-images.githubusercontent.com/104570014/166137415-2d9f799b-7944-4eb9-ac56-e90429eaf717.png)

- import project --> click on your project --> import project --> <br>
![image](https://user-images.githubusercontent.com/104570014/166137933-608f5a76-4f65-47ad-aece-eacb8df21c06.png)

  
 - left top corner click on action console --> click on your project --> Decide how your Action is invoked --> Enter Display Name (magic 8 ball ) --> Save <br>
![image](https://user-images.githubusercontent.com/104570014/166137967-a021d9d9-0192-4fa7-89de-46e30563e426.png) <br>

![image](https://user-images.githubusercontent.com/104570014/166138015-aae2c99e-b45c-4577-ba9d-56d5d90f3971.png)


![image](https://user-images.githubusercontent.com/104570014/166137627-910b77aa-a059-4dd3-8a56-9211de12c304.png)

![image](https://user-images.githubusercontent.com/104570014/166137654-7fe0e58a-0f61-405a-b37f-eb3966b4a5ad.png)


- Build your Action:
Go to Action console (Left top corner) --> click on your project --> Build your Action --> Add Action(s)
![image](https://user-images.githubusercontent.com/104570014/166138215-1b68b2bb-174c-4720-ba7f-70df2fe6de39.png)



--> Get Started --> Build --> Sign in with your account in the Dialogflow window(if needed) --> 

![image](https://user-images.githubusercontent.com/104570014/166138266-18cecaa8-af1a-4a5c-86f0-875c1c21fcd1.png)


![image](https://user-images.githubusercontent.com/104570014/166138293-14d2da1e-0a3e-4f08-96f4-89e25acf4778.png)

![image](https://user-images.githubusercontent.com/104570014/166138348-ca69af82-9674-4c0f-86d8-dabd1ede64b3.png)

create --> Left hand side there is an option call Fullfillment --> Webhook --> Enable --> Enter url --> here enter the trigger url of the cloud function created --> Save 

![image](https://user-images.githubusercontent.com/104570014/166138406-bb225433-c4a8-45e7-8a2c-6c68b5b8191f.png)

![image](https://user-images.githubusercontent.com/104570014/166138569-a7d3497d-5175-4af8-8a8f-fb6ab79e6042.png)

![image](https://user-images.githubusercontent.com/104570014/166138597-7bfb9aca-7419-4fcd-93b6-861d9444ef5d.png)

- Intents --> Click on "Default Welcome Intent" --> Responses --> Add Responses --> Text Response --> Enter "Welcome to the lab magic 8 ball, ask me a yes or no question and I will predict the future!"
as response --> Save

![image](https://user-images.githubusercontent.com/104570014/166138646-401cfae0-de9d-4c31-8baf-cf9a14004b75.png)


![image](https://user-images.githubusercontent.com/104570014/166138702-7ab13c0d-b445-40f8-8530-85240f1830ea.png)

- Intents --> Click on "Default Fallback Intent" --> Responses --> enable Set this intent as end of conversation --> Fulfillment --> enable Enable webhook call for this intent --> Save

![image](https://user-images.githubusercontent.com/104570014/166138750-774862cc-8732-4ba0-bb56-c497b7e28930.png)

![image](https://user-images.githubusercontent.com/104570014/166138790-0f8d3567-9414-4ef1-8136-72d880ff6262.png)

- Go to integration in the left panel click to Continue with the integration
![image](https://user-images.githubusercontent.com/104570014/166136751-95bd706a-f41f-4853-a9db-f847c580b7ca.png)

- click on test 
![image](https://user-images.githubusercontent.com/104570014/166136851-1c4a211c-2e6b-45ef-a479-3f570983491d.png)


You can test in Dialogflow to ensure it works.
![image](https://user-images.githubusercontent.com/104570014/166138857-849bcb52-a179-48a3-acab-e043e463717b.png)

![image](https://user-images.githubusercontent.com/104570014/166138864-fadf42e1-bb64-4d78-b68b-9feda96c38e1.png)


## Task 3: Add multilingual support to your Cloud Function

- Cloud Functions --> Trigger --> Edit --> Next --> Update the main.py file at line 15(line no. may differ) press enter add the code given in manual --> Deploy 

- Goto to Dialogflow window --> Test --> Test the sentences


# Congratulations you've completed your challenge lab
## Happy Learning
## See you in the cloud..

