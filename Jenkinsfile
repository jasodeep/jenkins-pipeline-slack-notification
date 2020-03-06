/*
       __   _______ .__   __.  __  ___  __  .__   __.      _______.    _______  __   __       _______ 
      |  | |   ____||  \ |  | |  |/  / |  | |  \ |  |     /       |   |   ____||  | |  |     |   ____|
      |  | |  |__   |   \|  | |  '  /  |  | |   \|  |    |   (----`   |  |__   |  | |  |     |  |__   
.--.  |  | |   __|  |  . `  | |    <   |  | |  . `  |     \   \       |   __|  |  | |  |     |   __|  
|  `--'  | |  |____ |  |\   | |  .  \  |  | |  |\   | .----)   |      |  |     |  | |  `----.|  |____ 
 \______/  |_______||__| \__| |__|\__\ |__| |__| \__| |_______/       |__|     |__| |_______||_______|                                                                                                                                              \|_________|                                                                                                                               
*/

/**
 * Send notification to slack channel
 *
 * @param status
 * @param message
 * @param color
 */
def sendSlackNotification(status, message, color) {

    URL url = new URL ("https://slack.com/api/chat.postMessage");
    HttpURLConnection con = (HttpURLConnection)url.openConnection();
    con.setRequestMethod("POST");
    con.setRequestProperty("Content-Type", "application/json; utf-8");
    con.setRequestProperty("Accept", "application/json");
    con.setRequestProperty("Authorization", "Bearer ${env.SLACK_TOKEN}");
    con.setDoOutput(true);            

    String jsonInputString = "{\"text\": \"\",\"channel\": \"${env.SLACK_CHANNEL}\", \"attachments\":  [{\"text\": \"${message}\",\"fields\":[{\"title\": \"Project\",\"value\": \"${currentBuild.projectName}\",\"short\": true},{\"title\": \"Environment\",\"value\": \"${env.ENVIROMENT}\",\"short\": true},{\"title\": \"Status\",\"value\": \"${currentBuild.currentResult}\",\"short\": true},{\"title\": \"Version\",\"value\": \"#${BUILD_NUMBER}\",\"short\": true}],\"color\": \"${color}\",\"footer\": \"Jenkins CI\",\"footer_icon\": \"http://ftp.yz.yamagata-u.ac.jp/pub/misc/jenkins/art/jenkins-logo/favicon.ico\",\"ts\": 1573241873}]}";

    OutputStream os = con.getOutputStream();
    byte[] input = jsonInputString.getBytes("utf-8");
    os.write(input, 0, input.length);			
            

    int code = con.getResponseCode();
    System.out.println(code);
            
    BufferedReader br = new BufferedReader(new InputStreamReader(con.getInputStream(), "utf-8"));
    StringBuilder response = new StringBuilder();
    String responseLine = null;
    while ((responseLine = br.readLine()) != null) {
        response.append(responseLine.trim());
    }
}

/* Pipeline Script*/
pipeline {
    agent any
    /* Enviroment variable initialization */
    environment {
        SLACK_TOKEN = "${SLACK_TOKEN}"
        SLACK_CHANNEL = "${SLACK_CHANNEL}"
        ENVIROMENT = "${SLACK_CHANNEL}"
    }
    
    stages {
        stage('BUILD') {
            steps {
                echo "Building ..."
                /** 
                 * build script
                */
            }
        }
        stage('TEST') {
            steps {
                echo "Running test cases ..."
                /** 
                 * run test cases  
                */
            }
        } 

        stage('DEPLOY') {
            steps {
                echo "Deploying ..."
                /** 
                 * deployment script
                */
            }
        } 
    }

    /* Post Deployment Steps */
    post {
        success {
            sendSlackNotification("SUCCESS", ":white_check_mark: _Build Successful_ :white_check_mark:", "#2EB886")
        }
        failure {
            sendSlackNotification('FAILURE', ":alert: _Something went wrong !_ :alert:", "#F35A00")
        }
    }
}