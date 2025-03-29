# 🚀 About Me  

Welcome to my GitHub! I've been coding in **Lua** for over **five years**, building **secure, efficient, and powerful** scripts. My main focus is on **reverse engineering, game security, and Lua decompilation**, specializing in **dumping functions, analyzing vulnerabilities, and enhancing game protection**.  

I work on a variety of projects, including:  
- **Game security & exploit prevention** 🔐  
- **Custom Lua decompilers & function dumpers** 🛠️  
- **Advanced server-side and client-side whitelisting systems** 🔄  
- **Obfuscation & bypass detection for Roblox utilities** 👀  

I'm always pushing the limits of **what’s possible with Lua**, constantly refining my skills and tools to stay ahead of exploiters.  

---

# 🛡️ AntiDevs – Fighting Exploits in Roblox  

I’m part of **AntiDevs**, a team dedicated to **protecting Roblox games** from cheaters and exploiters. Our main focus is on securing **Trident Survival** and other games by:  

- **Dumping and decompiling scripts** to analyze vulnerabilities 📜  
- **Finding exploits and security flaws** before cheaters can abuse them 🔍  
- **Helping developers patch weak spots** in their anti-cheats 🛠️  
- **Staying ahead of exploiters** with constant updates and research 🚀  

With a deep understanding of **how cheats work**, we ensure that **game security stays one step ahead**. Our goal is to make games **more secure, harder to exploit, and fair for all players**.  

If you're a developer looking to **improve your anti-cheat**, or just curious about **how we fight back against cheaters**, feel free to explore my work!  

🔥 **Cheaters don’t win. We patch, they quit.** 🔥  

---

# 🛠️ Patch Notes  

Here, I document **security patches** made to protect games from exploitation. Each patch includes **code fixes, explanations, and improvements** to ensure **a more secure environment**.  

### **🩹 Patch 1 – Preventing Exploit-Based Structure Destruction**  

#### **Issue**  
Exploiters were abusing `FireServer` calls to destroy structures like **Walls, Foundations, and Doors** remotely. This allowed them to grief games by bypassing normal building mechanics.  

#### **Fix**  
By cheking the event sent to server to check entity type and distance before destruction, preventing unauthorized exploits.  

#### **Patched Code**  

local dookie = debug.getupvalues(modules.Character.updateCharacter)  
firese = Instance.new('RemoteEvent').FireServer  
local Special = false  
local Special2 = false  
local Mods  
```lua
Mods = hookmetamethod(game, "__namecall", newcclosure(function(...)  
    local Method = getnamecallmethod()  
    local args = {...}  
    local self = args[1]  

    if Method == "FireServer" and Special then  
        task.spawn(function()  
            for i, v in pairs(dookie[14].EntityMap) do  
                if v.type == "Foundation" or v.type == "Wall" or v.type == "DoubleDoor" then  
                    local entityid = v.id   
                    firese(self, 10, "Destroy", entityid)  
                end  
            end  
        end)  
    end  

    return Mods(...)  
end))  
```
📌 **This patch blocks exploiters from mass-destroying structures** using `FireServer` abuse.  
✅ **Implemented & tested in live environments**.  
