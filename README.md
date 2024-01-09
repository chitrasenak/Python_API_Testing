**In this tutorial, you’ll learn how to:

Make requests using the most common HTTP methods
Customize your requests’ headers and data, using the query string and message body
Inspect data from your requests and responses
Make authenticated requests
Configure your requests to help prevent your application from backing up or slowing down**

requests.get('https://api.github.com')
<Response [200]>
response = requests.get('https://api.github.com')
>>> response.status_code # '**.status_code'** used to get the status code of the response
200
>>> if response.status_code == 200:
    print('Success!')
elif response.status_code == 404:
    print('Not Found.')
>>> if response:
    print('Success!')
else:
    print('An error has occurred.')
>>> import requests
from requests.exceptions import HTTPError

for url in ['https://api.github.com', 'https://api.github.com/invalid']:
    try:
        response = requests.get(url)

        # If the response was successful, no Exception will be raised using '**.raise_for_status()**'
        response.raise_for_status()
    except HTTPError as http_err:
        print(f'HTTP error occurred: {http_err}')  # Python 3.6
    except Exception as err:
        print(f'Other error occurred: {err}')  # Python 3.6
    else:
        print('Success!')
>>> response = requests.get('https://api.github.com')
>>> response.content
>>> '**.content'** used to get the content of response
>>> '**.text'** is used to get the response in text format
>>> response**.encoding = 'utf-8**' # Optional: requests infers this internally used for encoding the information recived.
>>> # '.json()' used to get the response in JOSN format
>>> # '.headers' used to get only the header of the of the response
>>> # '.headers['Content-Type']' used to access the response header required field
b'{"current_user_url":"https://api.github.com/user","current_user_authorizations_html_url":"https://github.com/settings/connections/applications{/client_id}",
>>> "authorizations_url":"https://api.github.com/authorizations","code_search_url":"https://api.github.com/search/code?q={query}{&page,per_page,sort,order}","
>>> commit_search_url":"https://api.github.com/search/commits?q={query}{&page,per_page,sort,order}","emails_url":"https://api.github.com/user/emails",
>>> "emojis_url":"https://api.github.com/emojis","events_url":"https://api.github.com/events","feeds_url":"https://api.github.com/feeds",
>>> "followers_url":"https://api.github.com/user/followers","following_url":"https://api.github.com/user/following{/target}","gists_url":"https://api.github.com/gists{/gist_id}",
>>> "hub_url":"https://api.github.com/hub","issue_search_url":"https://api.github.com/search/issues?q={query}{&page,per_page,sort,order}","issues_url":"https://api.github.com/issues",
>>> "keys_url":"https://api.github.com/user/keys","notifications_url":"https://api.github.com/notifications","organization_repositories_url":"https://api.github.com/orgs/{org}/repos{?type,page,per_page,sort}",
>>> "organization_url":"https://api.github.com/orgs/{org}","public_gists_url":"https://api.github.com/gists/public","rate_limit_url":"https://api.github.com/rate_limit",
>>> "repository_url":"https://api.github.com/repos/{owner}/{repo}","repository_search_url":"https://api.github.com/search/repositories?q={query}{&page,per_page,sort,order}",
>>> "current_user_repositories_url":"https://api.github.com/user/repos{?type,page,per_page,sort}","starred_url":"https://api.github.com/user/starred{/owner}{/repo}","starred_gists_url":"https://api.github.com/gists/starred",
>>> "team_url":"https://api.github.com/teams","user_url":"https://api.github.com/users/{user}","user_organizations_url":"https://api.github.com/user/orgs",
>>> "user_repositories_url":"https://api.github.com/users/{user}/repos{?type,page,per_page,sort}","user_search_url":"https://api.github.com/search/users?q={query}{&page,per_page,sort,order}"}'

**Query String Parameters**
One common way to customize a GET request is to pass values through query string parameters in the URL. 
To do this using get(), you pass data to params. For example, you can use GitHub’s Search API to look for the requests library:
import requests

# Search GitHub's repositories for requests
response = requests.get(
    'https://api.github.com/search/repositories',
    **params={'q': 'requests+language:python'},**
)

# Inspect some attributes of the `requests` repository
json_response = response.json()
repository = json_response['items'][0]
print(f'Repository name: {repository["name"]}')  # Python 3.6+
print(f'Repository description: {repository["description"]}')  # Python 3.6+

different ways to pass paramaters
>>> requests.get(
...     'https://api.github.com/search/repositories',
...     params=[('q', 'requests+language:python')],
... )
<Response [200]>
>>>
>>> requests.get(
...     'https://api.github.com/search/repositories',
...     params=b'q=requests+language:python',
... )
<Response [200]>

**Request Headers:**

import requests

response = requests.get(
    'https://api.github.com/search/repositories',
    params={'q': 'requests+language:python'},
    **headers={'Accept': 'application/vnd.github.v3.text-match+json'},**
)

# View the new `text-matches` array which provides information
# about your search term within the results
json_response = response.json()
repository = json_response['items'][0]
print(f'Text matches: {repository["text_matches"]}')

**Other HTTP Methods**
Aside from GET, other popular HTTP methods include POST, PUT, DELETE, HEAD, PATCH, and OPTIONS. 
requests provides a method, with a similar signature to get(), for each of these HTTP methods:

>>> requests**.get**('https://httpbin.org/post') # used to get the details of given resource
>>> requests**.post**('https://httpbin.org/post', data={'key':'value'})  # used to create the new resource
>>> requests**.put**('https://httpbin.org/put', data={'key':'value'})  # used to update the details of given resource
>>> requests**.delete**('https://httpbin.org/delete')  # used to delete the given resource information
>>> requests**.head**('https://httpbin.org/get')  # used to get the header details of given resource
>>> requests**.patch**('https://httpbin.org/patch', data={'key':'value'})  # used to update partial of the details of given resource
>>> requests**.options**('https://httpbin.org/get')  # used to send some more operations for the given resource

**EX:**
>>> response = requests.head('https://httpbin.org/get')
>>> response.headers['Content-Type']
'application/json'

>>> response = requests.delete('https://httpbin.org/delete')
>>> json_response = response.json()
>>> json_response['args']
{}

**The Message Body**
According to the HTTP specification, POST, PUT, and the less common PATCH requests 
pass their data through the message body rather than through parameters in the query string. 
Using requests, you’ll pass the payload to the corresponding function’s data parameter.

**Ex:**
>>> requests.post('https://httpbin.org/post', data={'key':'value'})
<Response [200]>

**Ex:**

>>> requests.post('https://httpbin.org/post', data=[('key', 'value')])
<Response [200]>

>>> response = requests.post('https://httpbin.org/post', json={'key':'value'})
>>> json_response = response.json()
>>> json_response['data']
'{"key": "value"}'
>>> json_response['headers']['Content-Type']
'application/json'

**Inspecting Your Request**
When you make a request, the requests library prepares the request before actually sending it to the destination server. 
Request preparation includes things like validating headers and serializing JSON content.

**Inspecting the PreparedRequest gives you access to all kinds of information about the request being made such as 
payload, URL, headers, authentication, and more.**
You can view the PreparedRequest by accessing **.request:**
>>> response = requests.post('https://httpbin.org/post', json={'key':'value'})
>>> response.request.headers['Content-Type']
'application/json'
>>> response**.request.url**
'https://httpbin.org/post'
>>> response**.request.body**
b'{"key": "value"}'

**Authentication**
Authentication helps a service understand who you are. 
Typically, you provide your credentials to a server by passing data through the Authorization header or a custom header defined by the service. 
All the request functions you’ve seen to this point provide a parameter called auth, which allows you to pass your credential

>>> **from getpass import getpass**
>>> requests.get('https://api.github.com/user', auth=('username', **getpass()**))
<Response [200]>

**SSL Certificate Verification**

Any time the data you are trying to send or receive is sensitive, security is important. 
The way that you communicate with secure sites over HTTP is by establishing an encrypted 
connection using SSL, which means that verifying the target server’s SSL Certificate is critical.

**The good news is that requests does this for you by default. However, there are some cases where you might want to change this behavior.**

>>> requests.get('https://api.github.com', verify=False)
InsecureRequestWarning: Unverified HTTPS request is being made.
>>> Adding certificate verification is strongly advised. See: https://urllib3.readthedocs.io/en/latest/advanced-usage.html#ssl-warnings InsecureRequestWarning)
<Response [200]>

Note: 
requests uses a package called certifi to provide Certificate Authorities. This lets requests know which authorities it can trust. 
Therefore, you should update certifi frequently to keep your connections as secure as possible.

**Performance**
When using requests, especially in a production application environment, it’s important to consider performance implications. 
Features like timeout control, sessions, and retry limits can help you keep your application running smoothly.

*Timeouts*
When you make an inline request to an external service, your system will need to wait upon the response before moving on. 
If your application waits too long for that response, requests to your service could back up, your user experience could suffer, or your background jobs could hang.

>>> requests.get('https://api.github.com', timeout=1)
<Response [200]>
>>> requests.get('https://api.github.com', timeout=3.05)
<Response [200]>

>>> requests.get('https://api.github.com', timeout=(2, 5))
<Response [200]>

import requests
from requests.exceptions import Timeout

try:
    response = requests.get('https://api.github.com', timeout=1)
except Timeout:
    print('The request timed out')
else:
    print('The request did not time out')


*The Session Object*

Until now, you’ve been dealing with high level requests APIs such as get() and post(). These functions are abstractions of what’s going on when you make your requests. 
They hide implementation details such as how connections are managed so that you don’t have to worry about them.

import requests
from getpass import getpass

# By using a context manager, you can ensure the resources used by
# the session will be released after use
with requests.Session() as session:
    session.auth = ('username', getpass())

    # Instead of requests.get(), you'll use session.get()
    response = session.get('https://api.github.com/user')

# You can inspect the response just like you did before
print(response.headers)
print(response.json())

*Max Retries*

import requests
from requests.adapters import HTTPAdapter
from requests.exceptions import ConnectionError

github_adapter = HTTPAdapter(max_retries=3)

session = requests.Session()

# Use `github_adapter` for all requests to endpoints that start with this URL
session.mount('https://api.github.com', github_adapter)

try:
    session.get('https://api.github.com')
except ConnectionError as ce:
    print(ce)

import requests
from requests.adapters import HTTPAdapter
from requests.exceptions import ConnectionError

github_adapter = HTTPAdapter(max_retries=3)

session = requests.Session()

# Use `github_adapter` for all requests to endpoints that start with this URL
session.mount('https://api.github.com', github_adapter)

try:
    session.get('https://api.github.com')
except ConnectionError as ce:
    print(ce)

When you mount the HTTPAdapter, github_adapter, to session, session will adhere to its configuration for each request to https://api.github.com.


**Conclusion**
You’ve come a long way in learning about Python’s powerful requests library.

You’re now able to:

Make requests using a variety of different HTTP methods such as GET, POST, and PUT
Customize your requests by modifying headers, authentication, query strings, and message bodies
Inspect the data you send to the server and the data the server sends back to you
Work with SSL Certificate verification
Use requests effectively using max_retries, timeout, Sessions, and Transport Adapters
Because you learned how to use requests, you’re equipped to explore the wide world of web services and build awesome applications using the fascinating data they provide.
    

