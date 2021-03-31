# spotify-save-discover-weekly

This script automatically saves your "Discover Weekly" playlist which is generated by Spotify and refreshed every Monday. The songs from your temporary playlist are saved into a permanent playlist, using the Spotify API ([Authorization Code Flow](https://developer.spotify.com/documentation/general/guides/authorization-guide/#authorization-code-flow)). The automation is powered by [Github Actions](https://docs.github.com/en/actions) and executes automatically on Mondays as defined in the [save.yaml](/.github/workflows/save.yaml).

## Initial Set Up (approx: 10 minutes)
You should not need to make any commits back to the repo. The files in [/setup](/setup) will help obtain the authorization information for setting up the environment variables in github secrets in order to allow `main.py` to execute properly.

### Spotify API Credentials
1. Fork this repo and open the `.sample.env` file from the [/setup](/setup) folder on your local machine. 
2. Sign into your [Spotify API Dashboard](https://developer.spotify.com/dashboard/applications) and create a new application.
3. Fill out the env file with the Client ID, Secret and Redirect URI details and save this file as `.env`. **Do not post these details anywhere publically.**

Example:
```
CLIENT_ID=thisisanid
CLIENT_SECRET=thisisasecret
REDIRECT_URI=https://github.com/RegsonDR
```
5. Execute [authorization.py](/setup/authorization.py) and open the URL generated (you may need to do a `pip install -r requirements` if you have any package errors). 
6. Authorize your app to access your Spotify account, this will then redirect you to a new url with a `?code=` parameter in the url.
7. Copy the whole url into the console and hit enter, this will then give you your refresh token. **Do not post this refresh token anywhere publically.**

Example:
 ```
$python authorization.py
Open this link in your browser: https://accounts.spotify.com/authorize?client_id=thisisanid&response_type=code&redirect_uri=https%3A%2F%2Fgithub.com%2FRegsonDR&scope=user-library-read+playlist-modify-public+playlist-modify-private+playlist-read-private+playlist-read-collaborative

Enter URL you was redirected to (after accepting authorization):
> https://github.com/RegsonDR?code=somecodehere

Your refresh token is: somerefreshtokenhere
```

### Github Actions
1. Go to the settings of your forked repo and click on secrets. 
2. You will need to create the following secrets:
  *  **CLIENT_ID** - Use the same Client ID from your `.env`.
  *  **CLIENT_SECRET** - Use the same Client Secret from your `.env`
  *  **REFRESH_TOKEN** - Use the refresh token generated in the [Spotify API Credentials](#spotify-api-credentials) instructions above.
  *  **DISCOVER_WEEKLY_ID** - This is the ID of your Discover Weekly playlist, which can obtained using the [method](#obtaining-spotify-playlist-ids) described below.
  *  **SAVE_TO_ID** - This is the ID of your permanent playlist, which can be obtained using the [method](#obtaining-spotify-playlist-ids) described below.

![image](https://user-images.githubusercontent.com/32569720/113211160-0a7d3380-926d-11eb-97bc-0e17ef911336.png)

### Obtaining Spotify Playlist IDs
1. After logging into Spotify, create a new playlist (or use an old playlist) where you would like the songs to be permanently stored.
2. Right click on this playlist > "Share" > Copy the Spotify URI (`spotify:playlist:c11M5VLWLMh66yW4gsl51S`). 
3. The ID is `c11M5VLWLMh66yW4gsl51S`.

Repeat these steps to get the Discover Weekly playlist's ID.

## Manually Execution
1. Go to Actions in your forked repo.
2. Click on "Save songs"
3. Click on "Run workflow" which will bring up a drop down menu.
4. Click on "Run Workflow" again, this will initiate the script. Within the next few minutes, the script should execute and your songs should be in your new playlist in spotify.

Any execution errors can be found from within the actions tab of your forked repo.

![image](https://user-images.githubusercontent.com/32569720/113211386-4fa16580-926d-11eb-94c9-ddb513a122a7.png)

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## License
[MIT](https://choosealicense.com/licenses/mit/)
