# BLAST
Official Discord bot of DISSOLVE server

import discord
import requests
import json
import random
import os
from keep_alive import keep_alive

client = discord.Client()

sad_words = ["sad","motivate", 'depressing','depressed', "sad",'bad']

starter_encouragements =["Cheer up my buoy!", " you are great"," Come on ma Buddy"]


def get_quote():
	response = requests.get("https://zenquotes.io/api/random")
	json_data=json.loads(response.text)
	quote = json_data[0]['q'] + " -" + json_data[0]['a']
	return(quote)

@client.event
async def on_ready():
	print('We have logged in as {0.user}'.format(client))

@client.event
async def on_message(message):
	if message.author == client.user:
		return

msg = message.content
if message.content.startswith('!inspire'):
	quote = get_quote()
	await message.channel.send(quote) 
if any(word in msg for word in sad_words):
	await message.channel.send(random.choice(starter_encouragements))

keep_alive()
client.run(os.getenv("TOKEN"))
