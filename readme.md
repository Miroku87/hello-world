# Weborama VPAID HTML5 v1.14.5

## Getting Started
To clone this repository run the command

```shell
git clone git@2.229.52.99:/home/git/repo/VPAID_HTML5.git
```

After the repository has been correctly cloned you must install the npm packages by running the command

```shell
npm install
```

Make sure you have the Grunt CLI installed before going on.
If you don't have it please follow the instructions on this page: [Grunt Install Instructions](http://gruntjs.com/getting-started)

After all the packages has been installed and you're sure you have the Grunt CLI you can build the project as said in the Build paragraph.

## Build
To build your VPAID run the following command while on the root of the project

```shell
grunt build
```

You will see a `dist.zip` file inside the root of the project.
You are now ready to traffick it inside Admire or WCM.

## Trafficking on Admire
To traffic the VPAID HTML5 on Admire you should follow these steps:
 1. Create your campaign
 2. Create a VAST VPAID Flight
 3. Upload your dist.zip file
 4. Set every file type to asset **except** for the `VPAIDAdLinear.min.js` one
 5. Edit the given XML like this:
  * The MediaFile tag should look like this (The important attributes are _type_ and _apiFramework_. It's highly advised to fill the values for _bitrate_, _delivery_, _scalable_, _width_ and _height_ as well, even if with fake values)
  ```XML
  <MediaFile id="0" maintainAspectRatio="true" bitrate="800" type="application/javascript" delivery="progressive" scalable="true" apiFramework="VPAID" width="640" height="360">
  ```
  * It's mandatory to add an `AdParameter` tag with a JSON inside. I recommend you to write it on a single line after the `VideoClicks` closing tag. Example:
  ```XML
  <AdParameters><![CDATA[{"clickThru":"http://media.adrcdn.com/ads/exit.html","clickTrackers":["http://evt.adrcntr.com/e/?id=84a5907a9017de718e602745ddbfec3d&ci=&ho=HOST&pl=default&cf=0&ff=0&el=video&rn=~RANDOM~&ob=default&ev=scrc&mo=0&fof=1&foe=1&foo=1&ms=&ca=0&rt=p"],"mediapath":"http://media.adrcdn.com/ads/Adrime/3130343734/112106/","duration":"00:00:10","id":"84a5907a9017de718e602745ddbfec3d","usePlugins":true,"videoName":"video.mp4","initialAudio":true,"VPAIDTabsPlugin":{"panels":[{"id":"descrizione","hasVideo":false,"videoName":"","feed":"none","gallery":"none"},{"id":"instagram","hasVideo":false,"videoName":"","feed":{"type":"instagram","token":"2304570340.0f095de.b76a9bf55d7d4ecb844dbe24df8451ba","webo_devid":"0f095de7f7864e08966c10a0c52122fa","from_userid":"2304570340","hashtag":"barilla"},"gallery":"none"},{"id":"video","hasVideo":true,"videoName":"video_2.mp4","feed":"none","gallery":"none"}]}}]]></AdParameters>
  ```
 6. From here behave like it's a normal VAST.
 7. Mind that actually Admire VAST Player does not support HTML5 VPAIDs.
  
## JSON Specifications
The JSON you will write inside the `AdParameter` tag must always have the following form and mandatory parameters:

```json
{
	"clickThru": "http://media.adrcdn.com/ads/exit.html",
	"clickTrackers": ["http://evt.adrcntr.com/e/?id=0f54c364534d698c50ac4c2de324cc72&ci=&ho=HOST&pl=default&cf=0&ff=0&el=video&rn=~RANDOM~&ob=default&ev=scrc&mo=0&fof=1&foe=1&foo=1&ms=&ca=0&rt=p"],
	"mediapath": "http://media.adrcdn.com/ads/Adrime/3130343734/112106/",
	"duration": "00:00:15",
	"id": "0f54c364534d698c50ac4c2de324cc72",
	"usePlugins": true
}
```

## Trafficking on WCM
Follow these steps
 1. Create a new VAST VPAID Creative
 2. Go to Elements
 3. Upload your dist.zip file
 4. Set every file as asset
 5. Go to Edit XML tab
 6. Edit the given XML like this:
  * The MediaFile tag should look like this (The important attributes are _type_ and _apiFramework_. It's highly advised to fill the values for _bitrate_, _delivery_, _scalable_, _width_ and _height_ as well, even if with fake values)
  ```XML
  <MediaFile id="0" maintainAspectRatio="true" bitrate="800" type="application/javascript" delivery="progressive" scalable="true" apiFramework="VPAID" width="640" height="360">
  ```
  * At the moment we can't give a JavaScript file the role of Media File so we'll have to do it manually by writing inside the MediaFile tag this:
  ```XML
  <![CDATA[~MEDIAPATH~VPAIDAdLinear.min.js]]>
  ```
  * It's mandatory to add an `AdParameter` tag with a JSON inside. I recommend you to write it on a single line after the `VideoClicks` closing tag. Example:
  ```XML
  <AdParameters><![CDATA[{ "clickThru":"~CLICK_1~", "clickTrackers":[], "mediapath":"~MEDIAPATH~", "duration":"15", "id": "~INSERTION_ID~", "host":"~HOST~", "a":{ "si":"~ACCOUNT_ID~", "te":"~TRACKING_ELEMENT_ID~", "aap":"~ASSIGNED_AD_POSITION_ID~", "agi":"~ACTION_GROUP_ID~", "ycp":"~CUSTOM_PARAMETER~"}, "usePlugins" : true, "showSkipAfter": 2, "showSkipCountdown": true, "videoName":"video.mp4"}]]></AdParameters>
  ```
 7. From here behave like it's a normal VAST.
 8. Mind that actually WCM VAST Player does not support HTML5 VPAIDs.

## Testing on the previewer
> Read this if you have CLI access to the previewer folder and if on the same machine there is NodeJS installed

To be able to test the VPAID HTML5 on the previewer for a start you must have the right site in the `/previewer/files/templates/XX/` folder (XX stands for your country code).
This site is available in the lib folder and is called `vast_vpaid_jw7_201511`. Take it and copy it in your country sites folder.

After this follow these steps:
 1. Open the previewer
 2. Start a new flight
 3. Upload a fake VPAIDAdLinear.js file
 4. Click on the `Edit` and `Preview` tabs (this way screenad files will be generated)
 5. Copy the `campaign/flight` numbers in the address bar
 6. Go on your shell
 7. Change directory to `/previewer/files/campaigns/~CAMPAIGN_ID~/~FLIGHT_ID~`
 8. Clone this repository as explained before but adding the name of the new folder
 
 ```shell
 git clone git@2.229.52.99:/home/git/repo/VPAID_HTML5.git temp
 ```
 9. Now you need the permissions to edit all the files so run this command (if you can't sudo please let an admin do that for you)
 
 ```shell
 sudo chgrp -r yourgroupname *
 sudo chmod -r 775 *
 ```
 10. What we need to do now is to copy the `.git` directory in our location so run this command
 
 ```shell
 mv temp/.git .
 ```
 11. If you now run a `git status` you'll see that Git thinks all the project files has been removed. To fix this run the following
 
 ```shell
 git reset --hard HEAD
 ```
 12. Repeat step 9
 13. Create a new branch to work on
 
 ```shell
 git checkout -b newbranch
 ```
 14. Install all Node packages
 
 ```shell
 npm install
 ```

If you followed correctly every step you should now have all the project files in the preview folder.
You can now edit what you want and when you are ready to preview you should just run the command

```shell
grunt buildPreview
```

and then open your preview from the previewer remembering to choose the `vast_vpaid_jw7` site from the dropdown menu.
