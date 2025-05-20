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

🔥 **Cheaters don’t win. We patch, they quit.** 🔥  

---

# 🛠️ Patch Notes  

Here, I document **security patches** made to protect games from exploitation. Each patch includes **the exploit code, technical explanation, and secure implementation** to ensure **a more protected environment**.  

---

### 🩹 Patch 1 – Preventing Exploit-Based Structure Destruction  

#### 📉 Issue  
Exploiters were abusing `FireServer` calls to destroy structures like **Walls, Foundations, and Doors** remotely. This allowed them to grief games by bypassing normal building mechanics and range checks.  

#### 🔍 Exploit Vector  
By hooking into the game and modifying arguments passed to the server, they iterated through all entities and sent `Destroy` events for every valid ID—regardless of distance or ownership.  

#### ✅ Fix  
We intercepted the `FireServer` call using `hookmetamethod`, checked that the target is a valid structure type, and ensured it's within an acceptable distance before allowing destruction.  

#### 🔐 Patched Code  

```lua
local firese = Instance.new('RemoteEvent').FireServer
local entity_list = debug.getupvalues(modules.Character.updateCharacter)
local special = false

local __destroy
__destroy = hookmetamethod(game, "__namecall", newcclosure(function(...)
    local args = {...}
    local self = args[1]
    local method = getnamecallmethod()

    if method == "FireServer" and special then
        task.spawn(function()
            for _, v in pairs(entity_list[14].EntityMap) do
                if v.type == "Foundation" or v.type == "Wall" or v.type == "DoubleDoor" then
                    local id = v.id
                    if is_within_range(id) then -- only allow nearby structures to be affected
                        firese(self, 10, "Destroy", id)
                    end
                end
            end
        end)
    end

    return __destroy(...)
end))
```

📌 **This patch blocks exploiters from mass-destroying structures** using remote abuse, by validating both structure type and proximity.  
✅ **Deployed & tested in live environments – confirmed effective.**

---

### 🧩 Upcoming Patches  
- **Patch 2 – Magic Bullet**: can’t share patch method yet! 🎯  
- **Patch 3 – Fast MiningDrill**: can’t share patch method yet! ⛏️  
- **Patch 4 – Mainstreamed Utility**: can’t share patch method yet! 🧰  

---

If you're a developer looking to **improve your anti-cheat**, or just curious about **how we fight back against cheaters**, feel free to explore my work!  
