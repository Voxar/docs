#Inter-App Audio for waking upp background application


Requirements 
===

Server application (the one being waked up)
---
* Add `inter-app-audio entitlement` to bundle id and application
* Add the `audio` background mode
* Add an AudioComponent plist description
* Create, initialize and publish a RemoteIO AudioComponentInstance
	
Client application (the in-focus application waking up the server)
---
* Add `inter-app-audio entitlement` to bundle id and application
* Add the `audio` background mode
* An active AVAudioSession
	* Can be set up with `MixWithOthers` category
* Create and initialize an AusioComponentInstance

Issues
===

Also see [http://lijon.github.io/iaa_quirks.html](http://lijon.github.io/iaa_quirks.html)


Client
---
** When client crashes with active IAA connection **

* IAA hosts can not connect again until the server is restarted and republishes the node which triggers the OS to clean up dead connections.
	* Repo: Start Server, Quit Server, Start Client, Connect successfully but not disconnect, Crash or Terminate from debugger (not triggering cleanup code), Start Client, Connect will fail with -1 timeout. 
* As client needs an active audio session to initialize the remote node IAA will not work when you can't get an audio session, such as phone call or using apps that does not allow other apps to play audio at the same time (for example SoloAmbient).


Impediments for third party app
===
* Needs the `inter-app-audio` entitlement
* Needs the `audio` background mode
* Needs to activate audio session
* May not crash while having an active connection as it requires restart of the **server**.
