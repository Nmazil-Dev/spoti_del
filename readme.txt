import os
import sys
import spotipy
import spotipy.util as util

#This is a small script that uses the spotipy library.
#It gets the currently playing song from spotify and removes it from your music library, then skips to the next track
#Enjoy cleaning out your music library! This works very well when mapped to an unused macro key on your keyboard.

def main():
	username = 'XXXXXXXXXXX'																 #Go to your Spotify profile and click the circle button>Share>Copy Spotify URI> spotify:user:XXXXXXXXXXX <-- Use these numbers only
	scope = "user-read-currently-playing user-library-modify user-modify-playback-state" 	 #Scopes used for the script separated by a space, you can find more of these on the Spotify Dev site
	CLIENT_ID = 'XXXXXXXXXXXXXXXXX' 														 #Get it from the Spotify Dev site on your app page
	CLIENT_SECRET = 'XXXXXXXXXXXXXXXXXXXX' 													 #Get it from the Spotify Dev site on your app page
	redirect_uri = "http://google.com/" 													 #The redirect url that you used when making your app (I used google)

	#Authenticate the token using spotipy
	token = util.prompt_for_user_token(username, scope, CLIENT_ID, CLIENT_SECRET, redirect_uri)

	#Spotify object
	sp = spotipy.Spotify(auth=token)

	#Gets the data for the currently playing song.
	results = sp.currently_playing()

	#Takes the song from the currently playing dictionary.
	song = results['item']

	#Gets the currently playing song's id.
	song_id = song['id']


	#Takes the song_id and puts it into a list, deletes the track from the music library, and skips to the next song.
	def del_song(song_id):
		del_id = []
		del_id.append(song_id)
		delete_track = sp.current_user_saved_tracks_delete(del_id)
		sp.next_track()



	del_song(song_id)


main()

