# Spotify API

This is a quick step-by-step guide on how to use the Spotify API to send or retrieve data using the Authorization Flow. 
To send HTTP Requests you can use any programming language of your choice or use [Reqbin](https://reqbin.com/) for testing purposes.

## Step One: Register with Spotify

Go to [https://developer.spotify.com/dashboard](https://developer.spotify.com/dashboard), log in, and create an application to receive your Client ID and Client Secret.


## Step 2: Redirect URI
Once Spotify has verified the user it sends back a code that is required for the next step. 

You need to set up a list of valid redirect URIs on the Spotify API Dashboard.
1. Under the dashboard select your application
2. Edit Settings
3. Enter your Redirect URI
4. Press Add
5. Save

The URI can be a locally hosted page or a website of your choosing or creation.
For Example
```
localhost:433
https://mydomain.com/
com.example.myapplication
```
For testing purposes, you can use [webhook.site](https://webhook.site/). Copy the Unique URL it provides and use that as your Redirect URI.

## Step 3
Navigate to this URL with your Client_ID, Redirect_URI & Your_Required_Scope swapped for your specific case. (Don't include the curly brackets)
```URL
https://accounts.spotify.com/authorize?client_id={Client_ID}&response_type=code&redirect_uri={Redirect_URI}&scope={Your_Required_Scope}
```
For this example, I will use the user-read-currently-playing scope to receive info on the currently playing song.

For a list of Spotify API Scopes click [here](https://developer.spotify.com/documentation/general/guides/authorization/scopes/).

You will them be prompted to log in to your Spotify Account and upon confirmation will be redirected to your Redirect_URI with a code at the end.
```
https://mydomain.com/?code=
```
## Step 4 Create your Authorisation ID

Use this [Online Javascript Compiler](https://www.programiz.com/javascript/online-compiler/) with the following code to generate your Authorisation ID.
```javascript
ClientID = "abcdefg";
ClientSecret= "1234567";
console.log((new Buffer( ClientID + ':' + ClientSecret ).toString('base64')));

```
Example Output:
```javascript
YWJjZGVmZzoxMjM0NTY3
```


## Step 5 Use the Code to request an API Token
### Step 5 Part 1: Request Token
Make an HTTP POST request to 
```
https://accounts.spotify.com/api/token
```
With the following contents (Fill in the code you received in the previous step and your redirect URI):
```
code=
redirect_uri=
grant_type=authorization_code

```

And the following Headers (Replace the Example Authorisation ID with your own):
```
Authorization: Basic YWJjZGVmZzoxMjM0NTY3
Content-Type: application/x-www-form-urlencoded
```

The server should respond with:
```json
{
    "access_token": "sensored_cus_its_private",
    "token_type": "Bearer",
    "expires_in": 3600,
    "refresh_token": "sensored_cus_its_also_private",
    "scope": "user-read-currently-playing"
}
```
The access_token has a validity of one hour and you can re-request a new token using the refresh_token parameter.

### Step 5 Part 2: Re-request Token 

To do this make an HTTP POST request to 
```
https://accounts.spotify.com/api/token
```
With the following contents (Fill in the code you received in the previous step and your redirect URI):
```
refresh_token=
grant_type=refresh_token

```

And the following Headers (Replace the Example Authorisation ID with your own):
```
Authorization: Basic YWJjZGVmZzoxMjM0NTY3
Content-Type: application/x-www-form-urlencoded
```

The server should respond with:
```json
{
    "access_token": "sensored_cus_its_private",
    "token_type": "Bearer",
    "expires_in": 3600,
    "scope": "user-read-currently-playing"
}
```

## Step 6
### Use Token to find Currently Playing Song

Make an HTTP GET request to 
```
https://api.spotify.com/v1/me/player/currently-playing
```
With the following Headers (Replace the Example Authorisation ID with your own):
```
Content-Type:application/json
Authorization:Bearer replace_with_token
```

The server should respond with:
```json
{
    "timestamp": 1677716564187,
    "context": {
        "external_urls": {
            "spotify": "https://open.spotify.com/collection/tracks"
        },
        "href": "https://api.spotify.com/v1/me/tracks",
        "type": "collection",
        "uri": "spotify:user:tag6r2nseudkwte6hpp8hjq51:collection"
    },
    "progress_ms": 46170,
    "item": {
        "album": {
            "album_type": "album",
            "artists": [{
                "external_urls": {
                    "spotify": "https://open.spotify.com/artist/0SiPm71q3iqjIKTwOq2guQ"
                },
                "href": "https://api.spotify.com/v1/artists/0SiPm71q3iqjIKTwOq2guQ",
                "id": "0SiPm71q3iqjIKTwOq2guQ",
                "name": "Cherish",
                "type": "artist",
                "uri": "spotify:artist:0SiPm71q3iqjIKTwOq2guQ"
            }],
            "available_markets": ["AD", "AE", "AG", "AL", "AM", "AO", "AR", "AT", "AU", "AZ", "BA", "BB", "BD", "BE", "BF", "BG", "BH", "BI", "BJ", "BN", "BO", "BR", "BS", "BT", "BW", "BY", "BZ", "CA", "CD", "CG", "CH", "CI", "CL", "CM", "CO", "CR", "CV", "CW", "CY", "CZ", "DE", "DJ", "DK", "DM", "DO", "DZ", "EC", "EE", "EG", "ES", "ET", "FI", "FJ", "FM", "FR", "GA", "GB", "GD", "GE", "GH", "GM", "GN", "GQ", "GR", "GT", "GW", "GY", "HK", "HN", "HR", "HT", "HU", "ID", "IE", "IL", "IN", "IQ", "IS", "IT", "JM", "JO", "KE", "KG", "KH", "KI", "KM", "KN", "KR", "KW", "KZ", "LA", "LB", "LC", "LI", "LK", "LR", "LS", "LT", "LU", "LV", "LY", "MA", "MC", "MD", "ME", "MG", "MH", "MK", "ML", "MN", "MO", "MR", "MT", "MU", "MV", "MW", "MX", "MY", "MZ", "NA", "NE", "NG", "NI", "NL", "NO", "NP", "NR", "NZ", "OM", "PA", "PE", "PG", "PH", "PK", "PL", "PS", "PT", "PW", "PY", "QA", "RO", "RS", "RW", "SA", "SB", "SC", "SE", "SG", "SI", "SK", "SL", "SM", "SN", "SR", "ST", "SV", "SZ", "TD", "TG", "TH", "TJ", "TL", "TN", "TO", "TR", "TT", "TV", "TW", "TZ", "UA", "UG", "US", "UY", "UZ", "VC", "VE", "VN", "VU", "WS", "XK", "ZA", "ZM", "ZW"],
            "external_urls": {
                "spotify": "https://open.spotify.com/album/4aOerKfjwm9zjkNJHC3iS5"
            },
            "href": "https://api.spotify.com/v1/albums/4aOerKfjwm9zjkNJHC3iS5",
            "id": "4aOerKfjwm9zjkNJHC3iS5",
            "images": [{
                "height": 640,
                "url": "https://i.scdn.co/image/ab67616d0000b273fce712500a4be6eaf874c20e",
                "width": 640
            }, {
                "height": 300,
                "url": "https://i.scdn.co/image/ab67616d00001e02fce712500a4be6eaf874c20e",
                "width": 300
            }, {
                "height": 64,
                "url": "https://i.scdn.co/image/ab67616d00004851fce712500a4be6eaf874c20e",
                "width": 64
            }],
            "name": "Best Collection '75",
            "release_date": "2022-12-07",
            "release_date_precision": "day",
            "total_tracks": 25,
            "type": "album",
            "uri": "spotify:album:4aOerKfjwm9zjkNJHC3iS5"
        },
        "artists": [{
            "external_urls": {
                "spotify": "https://open.spotify.com/artist/0SiPm71q3iqjIKTwOq2guQ"
            },
            "href": "https://api.spotify.com/v1/artists/0SiPm71q3iqjIKTwOq2guQ",
            "id": "0SiPm71q3iqjIKTwOq2guQ",
            "name": "Cherish",
            "type": "artist",
            "uri": "spotify:artist:0SiPm71q3iqjIKTwOq2guQ"
        }],
        "available_markets": ["AR", "AU", "AT", "BE", "BO", "BR", "BG", "CA", "CL", "CO", "CR", "CY", "CZ", "DK", "DO", "DE", "EC", "EE", "SV", "FI", "FR", "GR", "GT", "HN", "HK", "HU", "IS", "IE", "IT", "LV", "LT", "LU", "MY", "MT", "MX", "NL", "NZ", "NI", "NO", "PA", "PY", "PE", "PH", "PL", "PT", "SG", "SK", "ES", "SE", "CH", "TW", "TR", "UY", "US", "GB", "AD", "LI", "MC", "ID", "TH", "VN", "RO", "IL", "ZA", "SA", "AE", "BH", "QA", "OM", "KW", "EG", "MA", "DZ", "TN", "LB", "JO", "PS", "IN", "BY", "KZ", "MD", "UA", "AL", "BA", "HR", "ME", "MK", "RS", "SI", "KR", "BD", "PK", "LK", "GH", "KE", "NG", "TZ", "UG", "AG", "AM", "BS", "BB", "BZ", "BT", "BW", "BF", "CV", "CW", "DM", "FJ", "GM", "GE", "GD", "GW", "GY", "HT", "JM", "KI", "LS", "LR", "MW", "MV", "ML", "MH", "FM", "NA", "NR", "NE", "PW", "PG", "WS", "SM", "ST", "SN", "SC", "SL", "SB", "KN", "LC", "VC", "SR", "TL", "TO", "TT", "TV", "VU", "AZ", "BN", "BI", "KH", "CM", "TD", "KM", "GQ", "SZ", "GA", "GN", "KG", "LA", "MO", "MR", "MN", "NP", "RW", "TG", "UZ", "ZW", "BJ", "MG", "MU", "MZ", "AO", "CI", "DJ", "ZM", "CD", "CG", "IQ", "LY", "TJ", "VE", "ET", "XC", "XK"],
        "disc_number": 1,
        "duration_ms": 151933,
        "explicit": false,
        "external_ids": {
            "isrc": "JPVI02202281"
        },
        "external_urls": {
            "spotify": "https://open.spotify.com/track/0MzEpCmK8RYpsQPkz4mzKy"
        },
        "href": "https://api.spotify.com/v1/tracks/0MzEpCmK8RYpsQPkz4mzKy",
        "id": "0MzEpCmK8RYpsQPkz4mzKy",
        "is_local": false,
        "name": "Yume no Naka e",
        "popularity": 5,
        "preview_url": "https://p.scdn.co/mp3-preview/6d2b1e0e7fc0ecba0b3515e0b3c0703ec1f74d32?cid=deed877d94234beeb428d2cc0231288c",
        "track_number": 15,
        "type": "track",
        "uri": "spotify:track:0MzEpCmK8RYpsQPkz4mzKy"
    },
    "currently_playing_type": "track",
    "actions": {
        "disallows": {
            "resuming": true
        }
    },
    "is_playing": true
}
```
