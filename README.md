# GPA-Notifier
A shell script to send a notification with your latest GPA whenever it is uploaded.


```Shell
#!/bin/bash

echo
# Check cURL command if available (required), abort if does not exists
type curl >/dev/null 2>&1 || { echo >&2 "Required curl but it's not installed. Aborting."; exit 1; }
echo

echo "Enter your Registration Number"
read regno

echo "Enter your SLCM Password"
read password

clear

PAYLOAD='{"username": "'$regno'", "password": "'$password'"}'

while true; 
do 

RESPONSE=`curl -s --request POST -H "Content-Type:application/json" http://slcm.nmnjn.me/gpa2 --data "${PAYLOAD}"`
GPA=`echo "$RESPONSE" | grep -Eo '[0-9]+([.][0-9]+)?'`

echo
date

if [ -z "$GPA" ];
then
    echo "Unable to fetch GPA for this account ** please check the credentials **"

elif [[ $(bc <<< "$GPA > 0.00 && $GPA <= 10.00") == 1 ]]
then
    echo "GPA is ${GPA}"

    #un-comment the next line if you are using mac-os
    #osascript -e 'display notification "Your latest GPA is '$GPA'" with title "Death Alert"'

    #un-comment the next line if you are using linux
    #notify-send -u critical "Your latest GPA is '$GPA'"

else
    echo "GPA is not uploaded yet 🥳"
fi

sleep 60
done
```

# Running the Script
Save the above code in a shell script `.sh`, or download from the repository, and run it in your terminal window.

**You will need to give the executable permission to the script.** 

Run the following command: `chmod +x ohno.sh`

---

You can change how frequently the GPA is checked by changing the number of `sleep seconds` in the script. 

By default it is `60 seconds` i.e the GPA is checked every minute.

🌟


