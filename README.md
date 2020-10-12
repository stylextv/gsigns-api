![Cover Art](https://raw.githubusercontent.com/StylexTV/GSigns-API/main/showcase/cover/cover.png)

# 🔓 GSigns-API
[![stars](https://img.shields.io/github/stars/StylexTV/GSigns-API.svg?color=ffdd00)](https://GitHub.com/StylexTV/GSigns-API/stargazers/)
[![Codacy Badge](https://app.codacy.com/project/badge/Grade/a33dbb19ff17460d896a7864fececab6)](https://www.codacy.com/manual/noluck942/GSigns?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=StylexTV/GSigns&amp;utm_campaign=Badge_Grade)

GSigns-API is a lightweight api for the plugin GSigns that allows the creation and removal of signs.
> The github page of the plugin can be found [here](https://github.com/StylexTV/GSigns/).


## Including

To include this api in your own plugin, download the [latest release](https://github.com/StylexTV/GSigns-API/releases/) and add the following line to your `plugin.yml`:
```bash
depend: [GSigns]
```


## Implementing GSigns

Implementing GSigns is quite simple. It requires getting the GSigns-API service from the Bukkit ServiceManager. See the example below:
```java
package com.example.plugin;

import java.util.UUID;

import org.bukkit.Bukkit;
import org.bukkit.Location;
import org.bukkit.World;
import org.bukkit.plugin.RegisteredServiceProvider;
import org.bukkit.plugin.java.JavaPlugin;
import org.bukkit.scheduler.BukkitRunnable;

import de.stylextv.gs.api.GSignsAPI;

public class Main extends JavaPlugin {
	
	private GSignsAPI gSignsAPI;
	
	private UUID signUid;
	
	@Override
	public void onEnable() {
		if(!setupGSignsAPI()) {
			System.out.println(getDescription().getName()+" - Disabled due to no GSigns dependency found!");
			getServer().getPluginManager().disablePlugin(this);
			return;
        	}
		
		new BukkitRunnable() {
			@Override
			public void run() {
				World w = Bukkit.getWorld("world");
				String code = "{bg-url:https://raw.githubusercontent.com/StylexTV/GSigns/master/showcase/socials/cover.png,dith:false}";
				signUid = gSignsAPI.createSign(code, new Location(w, -402, 100, -43), new Location(w, -395, 97, -43));
			}
		}.runTaskLater(this, 5);
	}
	
	@Override
	public void onDisable() {
		if(signUid != null) {
			gSignsAPI.removeSign(signUid);
		}
	}
	
	private boolean setupGSignsAPI() {
        	if(getServer().getPluginManager().getPlugin("GSigns") == null) {
        		return false;
        	}
        	RegisteredServiceProvider<GSignsAPI> rsp = getServer().getServicesManager().getRegistration(GSignsAPI.class);
        	if(rsp == null) {
			return false;
		}
		gSignsAPI = rsp.getProvider();
		return gSignsAPI != null;
	}
	
}
```
