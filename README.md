i have a ctf and create a bash script to test a urls file for openredirect using these parametrs and the payload is eval.com


?next=
?url=
?target=
?rurl=
?dest=
?destination=
?redir=
?redirect_uri=
?redirect_url=
?redirect=
/redirect/
/cgi-bin/redirect.cgi?
/out/
/out?
?view=
/login?to=
?image_url=
?go=
?return=
?returnTo=
?return_to=
?checkout_url=
?continue=
?return_path=


Sure, you can use a simple Bash script to test URLs for open redirects using the specified parameters and payload. Here's a basic example:
---------------------------------------------------

bash
Copy code
#!/bin/bash

# Specify the payload
payload="eval.com"

# Read URLs from a file (one URL per line)
while IFS= read -r url; do
    # Test each parameter
    for param in "next" "url" "target" "rurl" "dest" "destination" "redir" "redirect_uri" "redirect_url" "redirect" "redirect/" "cgi-bin/redirect.cgi?" "out/" "out?" "view" "login?to" "image_url" "go" "return" "returnTo" "return_to" "checkout_url" "continue" "return_path"; do
        # Construct the full URL with the parameter and payload
        test_url="${url}?${param}=${payload}"

        # Use curl to make a request and check for the payload in the response
        response=$(curl -s -L -I -o /dev/null -w "%{http_code}" "$test_url")

        # Check if the response contains the payload
        if [[ $response == *"eval.com"* ]]; then
            echo "Vulnerable URL: $test_url"
            break
        fi
    done
done < "urls.txt"

------------------------------------------------
Make sure to replace "urls.txt" with the actual file containing your list of URLs. This script reads each URL from the file and appends each parameter with the specified payload ("eval.com"). If the response contains the payload, it indicates a potential open redirect vulnerability.

Note: This script is a basic example, and in a real-world scenario, you might need to consider additional factors, such as handling redirects properly, dealing with different HTTP methods, and more thorough analysis of the responses. Also, keep in mind that testing URLs without proper authorization may violate terms of service or even be illegal. Always ensure you have the right permissions before conducting such tests.
