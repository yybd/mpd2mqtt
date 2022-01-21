# mpd2mqtt

git:

    cd /home
    git clone https://github.com/yybd/mpd2mqtt.git
  
  
install:

    apt update && apt -y upgrade; apt install -y mpc iputils-ping jq mosquitto-clients; apt autoremove -y; rm -rf /var/lib/apt/lists

set Permission:

    chmod 744 /home/mpd2mqtt/mpd2mqtt.sh
    
 config mqtt in data/mpd2mqtt.config file:
 
    mpd_server='localhost'
    mpd_password=''
    mpd_port=''
    mqtt_server='10.0.1.12' <you ip mqtt server>
    mqtt_topic_get='musicSalon/mpd/get'
    mqtt_topic_set='musicSalon/mpd/set'
    mqtt_user=''
    mqtt_password=''
    debug='0'

copy mpd2mqtt.service to /lib/systemd/system & enable service:

    cp mpd2mqtt.service /lib/systemd/system

    sudo systemctl daemon-reload
    sudo systemctl enable mpd2mqtt

    systemctl start mpd2mqtt
    
    systemctl status mpd2mqtt


# mqtt client publisher & subscribe

You can change the payers state by, publisher to the topic music/mpd/set.

    {"player": "play"}
    {"player": "pause"}
    {"player": "toggle"}
    {"player": "next"}
    {"player": "prev"}
    {"player": "stop"}
    {"player": "update"}

It's possible to change options by:

    {"options": { "random": "on"} }
    {"options": { "replaygain": "track"} }
    {"options": { "consume": "off"} }
    {"options": { "repeat": "on"} }
    {"options": { "single": "off"} }
    {"options": { "volume": "+3"} }

You can change your queue:

    {"queue": { { "clear": "true", "add": "MixedMusic/preferredSongs", "play": "true" } } }
    {"queue": { { "clear": "false", "del": "3" } } }
    {"queue": { { "clear": "false", "insert": "MixedMusic/preferredSongs/I want to hear next.mp3", "play": "true" } } }
    
 The current options of MPD will be send in a message like:

    {
      "options": {
        "volume": "98%",
        "repeat": "on",
        "random": "off",
        "single": "off",
        "consume": "off"
      }

subscribe the topic music/mpd/get:
    
    {
      "player": {
        "current": {
          "name": "",
          "artist": "Billy Talent",
          "album": "Billy Talent III",
          "albumartist": "",
          "comment": "",
          "composer": "",
          "date": "2009",
          "originaldate": "%originaldate%",
          "disc": "",
          "genre": "",
          "performer": "",
          "title": "Saint Veronika",
          "track": "",
          "time": "4:10",
          "file": "Alben/Billy Talent - Billy Talent III/03 - Billy Talent - Saint Veronika.mp3",
          "position": "16",
          "id": "67",
          "prio": "0",
          "mtime": "Mon Dec 27 16:42:15 2010",
          "mdate": "12/27/10"
        },
        "state": "paused"
      }
      


To debug or check it you can use the commandline mqtt client: 

    mosquitto_pub -h localhost -t "music/mpd/set" -m '{"player":"toggle"}'



