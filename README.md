

task.wait(2)
repeat  task.wait() until game:IsLoaded()
if game.PlaceId == 8304191830 then
    repeat task.wait() until game.Workspace:FindFirstChild(game.Players.LocalPlayer.Name)
    repeat task.wait() until game.Players.LocalPlayer.PlayerGui:FindFirstChild("collection"):FindFirstChild("grid"):FindFirstChild("List"):FindFirstChild("Outer"):FindFirstChild("UnitFrames")
else
    repeat task.wait() until game.Workspace:FindFirstChild(game.Players.LocalPlayer.Name)
    game:GetService("ReplicatedStorage").endpoints.client_to_server.vote_start:InvokeServer()
    repeat task.wait() until game:GetService("Workspace")["_waves_started"].Value == true
end



local HttpService = game:GetService("HttpService")
local RunService = game:GetService("RunService")
local mouse = game.Players.LocalPlayer:GetMouse()
local UserInputService = game:GetService("UserInputService")

getgenv().savefilename = "AnimeAdventures_"..game.Players.LocalPlayer.Name..".json"

function sex()
    -- reads jsonfile
    local jsonData = readfile(savefilename)
    local data = HttpService:JSONDecode(jsonData)

    -- global values
    getgenv().disableatuofarm = false

    getgenv().sellatwave = data.sellatwave 
    getgenv().autosell = data.autosell
    getgenv().AutoFarm = data.autofarm
    getgenv().weburl = data.webhook
    -- getgenv().unitname = data.unitname
    -- getgenv().unitid = data.unitid
    getgenv().autostart = data.autostart
    getgenv().autoupgrade = data.autoupgrade
    getgenv().difficulty = data.difficulty
    getgenv().world = data.world
    getgenv().level = data.level
    getgenv().door = data.door

    getgenv().SpawnUnitPos = data.xspawnUnitPos
    getgenv().SelectedUnits = data.xselectedUnits

    ---// updates the json file
    function updatejson()

        local xdata = {
            -- unitname = getgenv().unitname,
            -- unitid = getgenv().unitid,
            sellatwave = getgenv().sellatwave,
            autosell = getgenv().autosell,
            webhook = getgenv().weburl,
            autofarm = getgenv().AutoFarm,
            autostart = getgenv().autostart,
            autoupgrade = getgenv().autoupgrade,
            difficulty = getgenv().difficulty,
            world = getgenv().world,
            level = getgenv().level,
            door = getgenv().door,
            xspawnUnitPos = getgenv().SpawnUnitPos,
            xselectedUnits = getgenv().SelectedUnits
        }

        local json = HttpService:JSONEncode(xdata)
        writefile(savefilename, json)

    end

    --------------------------------------------------
    --------------------------------------------------
    -- Uilib Shits
    local DiscordLib = loadstring(game:HttpGet "https://pastebin.com/raw/1kUyp5Ps")()
    local win = DiscordLib:Window("[ Wallay Hub ] Anime Adventures v 1.0".." - "..tostring(identifyexecutor()))
    local serv = win:Server(">> MAIN <<", "http://www.roblox.com/asset/?id=6031075938")

    local unitinfo



    function webhook()
        pcall(function()
            gems = tostring(game:GetService("Players").LocalPlayer.PlayerGui.ResultsUI.Holder.GoldGemXP.GemReward.Main.Amount.Text)
            cwaves = game:GetService("Players").LocalPlayer.PlayerGui.ResultsUI.Holder.Middle.WavesCompleted.Text
            ctime = game:GetService("Players").LocalPlayer.PlayerGui.ResultsUI.Holder.Middle.Timer.Text
            waves = cwaves:split(": ")
            ttime = ctime:split(": ")

            local url = tostring(getgenv().weburl) --webhook
            print(debug.traceback())
            local data = {
                ["content"] = "",
                ["username"] = "Anime Adventures",
                ["avatar_url"] = "https://tr.rbxcdn.com/e5b5844fb26df605986b94d87384f5fb/150/150/Image/Jpeg",
                ["embeds"] = {
                    {
                        ["author"] = {
                            ["name"] = "Wallay Hub Anime Adventures | Result ✔",
                            ["icon_url"] = "https://cdn.discordapp.com/emojis/997123585476927558.webp?size=96&quality=lossless"
                        },
                        ["description"] = "🎮 **"..game:GetService("Players").LocalPlayer.Name.."** 🎮",
                        ["color"] = 110335,

                        ["thumbnail"] = {
                            ['url'] = "https://www.roblox.com/headshot-thumbnail/image?userId=" ..
                                game.Players.LocalPlayer.userId ..
                                "&width=420&height=420&format=png"
                        },

                        ["fields"] = {
                            {
                                ["name"] = "Waves ที่มึงไปได้:",
                                ["value"] = tostring(waves[2]) ..
                                    " <:wave:997136622363627530>",
                                ["inline"] = true
                            }, {
                                ["name"] = "Gems ที่มึ้งได้:",
                                ["value"] = gems .. " <:gem:997123585476927558>",
                                ["inline"] = true
                            }, {
                                ["name"] = "เวลาที่มึงใช้ลง:",
                                ["value"] = tostring(ttime[2]) .. " ⏳",
                                ["inline"] = true
                            }
                        }
                    }
                }
            }

            local porn = game:GetService("HttpService"):JSONEncode(data)

            local headers = {["content-type"] = "application/json"}
            request = http_request or request or HttpPost or syn.request or http.request
            local sex = {Url = url, Body = porn, Method = "POST", Headers = headers}
            warn("Sending webhook notification...")
            request(sex)
        end)

    end

            
    if game.PlaceId == 8304191830 then
        local unitselecttab = serv:Channel("เลือกตัวละคร")
        local autofarmtab = serv:Channel("เล่นอัตโนมัต")
        --------------------------------------------------
        --------------- Select Units Tab -----------------
        --------------------------------------------------

        local Units = {}

        function loadUnit()
            repeat task.wait() until game.Players.LocalPlayer.PlayerGui:FindFirstChild("collection"):FindFirstChild("grid"):FindFirstChild("List"):FindFirstChild("Outer"):FindFirstChild("UnitFrames")
            task.wait(2)
            table.clear(Units)
            for i, v in pairs(game:GetService("Players")[game.Players.LocalPlayer.Name].PlayerGui.collection.grid.List.Outer.UnitFrames:GetChildren()) do
                if v.Name == "CollectionUnitFrame" then
                    repeat task.wait() until v:FindFirstChild("_uuid")
                    table.insert(Units, v.name.Text .. " #" .. v._uuid.Value)
                end
            end
        end

        loadUnit()

        function Equip()
            game:GetService("ReplicatedStorage").endpoints.client_to_server.unequip_all:InvokeServer()
            for i = 1, 4 do
                local unitinfo = getgenv().SelectedUnits["U" .. i]
                warn(unitinfo)
                if unitinfo ~= nil then
                    local unitinfo_ = unitinfo:split(" #")
                    task.wait(0.3)
                    game:GetService("ReplicatedStorage").endpoints.client_to_server.equip_unit:InvokeServer(unitinfo_[2])
                end
            end
            updatejson()
        end

        local drop = unitselecttab:Dropdown("ตัวละครที่ 1", Units, function(bool)
            getgenv().SelectedUnits["U1"] = bool
            --print("U1", bool, getgenv().SelectedUnits["U1"])
            Equip()
        end) -- select unit dropdown end

        local drop2 = unitselecttab:Dropdown("ตัวละครที่ 2", Units, function(bool)
            getgenv().SelectedUnits["U2"] = bool
            --print("U2", bool, getgenv().SelectedUnits["U2"])
            Equip()
        end)

        local drop3 = unitselecttab:Dropdown("ตัวละครที่ 3", Units, function(bool)
            getgenv().SelectedUnits["U3"] = bool
            --print("U3", bool, getgenv().SelectedUnits["U3"])
            Equip()
        end)

        local drop4 = unitselecttab:Dropdown("ตัวละครที่ 4", Units, function(bool)
            getgenv().SelectedUnits["U4"] = bool
            --print("U4", bool, getgenv().SelectedUnits["U4"])
            Equip()
        end)
        --------------// Refresh Unit List \\-------------

        unitselecttab:Button("รีเซ็ตตัวละคร [ Table ]", function()
            drop:Clear() -- clears list
            drop2:Clear()
            drop3:Clear()
            drop4:Clear()
            loadUnit()
            game:GetService("ReplicatedStorage").endpoints.client_to_server.unequip_all:InvokeServer()
            for i, v in ipairs(Units) do
                drop:Add(v)
                drop2:Add(v)
                drop3:Add(v)
                drop4:Add(v)
            end
            getgenv().SelectedUnits = {
                U1 = nil,
                U2 = nil,
                U3 = nil,
                U4 = nil
            }
        end) -- refresh list end
        unitselecttab:Label(" ")
        unitselecttab:Label(" ")

        --------------------------------------------------
        ------------------ Auto Farm Tab -----------------
        --------------------------------------------------
        
        autofarmtab:Textbox("แจ้งเตือนผ่านดิสขอด", "ใส่ลิ้งดิสขอด", false, function(web_url)
            getgenv().weburl = web_url
            updatejson()
        end)

        autofarmtab:Toggle("เล่นอัตโนมัต", getgenv().AutoFarm, function(bool)
            getgenv().AutoFarm = bool
            updatejson()
        end)

        getgenv().levels = {"nill"}

        autofarmtab:Toggle("เริ่มเกมส์อัตโนมัต", getgenv().autostart, function(bool)
            getgenv().autostart = bool
            updatejson()

            if getgenv().autostart and getgenv().AutoFarm then

                for i, v in pairs(game:GetService("Workspace")["_LOBBIES"].Story:GetDescendants()) do
                    if v.Name == "Owner" and v.Value == nil then
                        getgenv().door = v.Parent.Name
                        break
                    end
                end

                task.wait(0.1)

                local args = {
                    [1] = getgenv().door
                }
                game:GetService("ReplicatedStorage").endpoints.client_to_server.request_join_lobby:InvokeServer(unpack(
                    args))

                    task.wait(0.1)

                local args = {
                    [1] = getgenv().door, -- Lobby 
                    [2] = getgenv().level, -- World
                    [3] = true, -- Friends Only or not
                    [4] = getgenv().difficulty
                }
                game:GetService("ReplicatedStorage").endpoints.client_to_server.request_lock_level:InvokeServer(unpack(
                    args))

                    task.wait(0.1)

                local args = {
                    [1] = getgenv().door
                }
                game:GetService("ReplicatedStorage").endpoints.client_to_server.request_start_game:InvokeServer(unpack(
                    args))
            end

        end)
        
        autofarmtab:Toggle("อัพเกรดตัวละครอัตโนมัต", getgenv().autoupgrade, function(bool)
            getgenv().autoupgrade = bool
            updatejson()
        end)

        autofarmtab:Toggle("ขายตัวทึ้งอัตโนมัต", getgenv().autosell, function(x)
            getgenv().autosell = x
            updatejson()
            if getgenv().autosell == false then
                getgenv().disableatuofarm = false
            end
        end)

        autofarmtab:Textbox("ใส่ตัวเลข Wave ที่จะขายตัว", "ตัวเลข Wave", false, function(t)
            getgenv().sellatwave = tonumber(t)
            updatejson()
        end)

        

        local worlddrop = autofarmtab:Dropdown("เลือกแมพ", {"Plannet Namak", "Shiganshinu District", "Snowy Town","Hidden Sand Village"}, function(world)
            getgenv().world = world
            updatejson()
            if world == "Plannet Namak" then
                getgenv().leveldrop:Clear()
                table.clear(levels)
                getgenv().levels = {"namek_infinite", "namek_level_1", "namek_level_2", "namek_level_3",
                                    "namek_level_4", "namek_level_5", "namek_level_6"}
                for i, v in ipairs(levels) do
                    getgenv().leveldrop:Add(v)
                end
            elseif world == "Shiganshinu District" then
                getgenv().leveldrop:Clear()
                table.clear(levels)
                getgenv().levels = {"aot_infinite", "aot_level_1", "aot_level_2", "aot_level_3", "aot_level_4",
                                    "aot_level_5", "aot_level_6"}
                for i, v in ipairs(levels) do
                    getgenv().leveldrop:Add(v)
                end
            elseif world == "Snowy Town" then
                getgenv().leveldrop:Clear()
                table.clear(levels)
                getgenv().levels = {"demonslayer_infinite", "demonslayer_level_1", "demonslayer_level_2",
                                    "demonslayer_level_3", "demonslayer_level_4", "demonslayer_level_5",
                                    "demonslayer_level_6"}
                for i, v in ipairs(levels) do
                    getgenv().leveldrop:Add(v)
                end
            elseif world == "Hidden Sand Village" then
                getgenv().leveldrop:Clear()
                table.clear(levels)
                getgenv().levels = {"naruto_infinite", "naruto_level_1", "naruto_level_2", "naruto_level_3",
                                    "naruto_level_4", "naruto_level_5", "naruto_level_6"}
                for i, v in ipairs(levels) do
                    getgenv().leveldrop:Add(v)
                end
            end
        end)

        getgenv().leveldrop = autofarmtab:Dropdown("เลือก Level", getgenv().levels, function(level)
            getgenv().level = level
            updatejson()
        end)

        getgenv().diff = autofarmtab:Dropdown("เลือก ความยาก", {"Normal", "Hard"}, function(diff)
            getgenv().difficulty = diff
            updatejson()
        end)
        autofarmtab:Label(" ")
        autofarmtab:Label(" ")

    else -- When in a match
        local autofarmtab = serv:Channel("เล่นอัตโนมัต")
        local autoseltab = serv:Channel("ขายอัตโนมัต")

        game.Players.LocalPlayer.PlayerGui.MessageGui.Enabled = false
        game:GetService("ReplicatedStorage").packages.assets["ui_sfx"].error.Volume = 0
        game:GetService("ReplicatedStorage").packages.assets["ui_sfx"].error_old.Volume = 0

        autofarmtab:Textbox("ใส้ลิ้งWebhook ดิสขอด", "ลิ้งดิส Webhook ", false, function(web_url)
            getgenv().weburl = web_url
            updatejson()
        end)
        autofarmtab:Button("ทดสอบ webhook", function()
            webhook()
        end)


        autofarmtab:Toggle("เล่นอัตโนมัต", getgenv().AutoFarm, function(bool)
            getgenv().AutoFarm = bool
            updatejson()
        end)
        autofarmtab:Toggle("เริ่มเกมส์อัตโนมัต", getgenv().autostart, function(bool)
            getgenv().autostart = bool
            updatejson()
        end)

        autoseltab:Toggle("ขายตัวทึ้งอัตโนมัต", getgenv().autosell, function(x)
            getgenv().autosell = x
            updatejson()
            if getgenv().autosell == false then
                getgenv().disableatuofarm = false
            end

        end)

        autoseltab:Textbox("Wave ที่จะขายตัว", "ตัวเลข", false, function(t)
            getgenv().sellatwave = tonumber(t)
            updatejson()
        end)

        autofarmtab:Toggle("อัพเกรดตัวละครอัตโนมัต", getgenv().autoupgrade, function(bool)
            getgenv().autoupgrade = bool
            updatejson()
        end)

        function MouseClick(UnitPos)
            local connection
            connection = UserInputService.InputBegan:Connect(
                function(input, gameProcessed)
                    if input.UserInputType == Enum.UserInputType.MouseButton1 then
                        connection:Disconnect()
                        local a = Instance.new("Part", game.Workspace)
                        a.Size = Vector3.new(1, 1, 1)
                        a.Material = Enum.Material.Neon
                        a.Position = mouse.hit.p
                        task.wait()
                        a.Anchored = true
                        DiscordLib:Notification("ตําแหน่งจุดวางตัว:", tostring(a.Position), "ตกลง")
                        a.CanCollide = false
                        for i = 0, 1, 0.1 do
                            a.Transparency = i
                            task.wait()
                        end
                        a:Destroy()
                        SpawnUnitPos[UnitPos]["x"] = a.Position.X
                        SpawnUnitPos[UnitPos]["y"] = a.Position.Y
                        SpawnUnitPos[UnitPos]["z"] = a.Position.Z
                        updatejson()
                    end
                end)
        end

        -- // Set Position
        autofarmtab:Button("ตั้งค่าจุดวางตัวที่ 1", function()
            DiscordLib:Notification("Set Position for Unit 1 to Spawn",
                "ตั้งค่าจุดวางตัว ที่ 1 เรียบร้อยแล้ว",
                "ตกลง")
            MouseClick("UP1")
        end)

        autofarmtab:Button("ตั้งค่าจุดวางตัวที่ 2", function()
            DiscordLib:Notification("Set Position for Unit 2 to Spawn",
                "ตั้งค่าจุดวางตัวที่ 2 เรียบร้อยแล้ว",
                "ตกลง")
            MouseClick("UP2")
        end)
        autofarmtab:Button("ตั้งค่าจุดวางตัวที่ 3", function()
            DiscordLib:Notification("Set Position for Unit 3 to Spawn",
                "ตั้งค่าจุดวางตัวที่ 3 เรียบร้อยแล้ว",
                "ตกลง")
            MouseClick("UP3")
        end)
        autofarmtab:Button("ตั้งค่าจุดวางตัวที่ 4", function()
            DiscordLib:Notification("Set Position for Unit 4 to Spawn",
                "ตั้งค่าจุดวางตัวที่ 4 เรียบร้อยแล้ว",
                "ตกลง")
            MouseClick("UP4")
        end)

        -- set unit position end--
        autofarmtab:Label("--- จดจําการตั้งค่า ---")
        autofarmtab:Label("ขายตัวทึ้งอัตโนมัต Wave: " .. tostring(getgenv().sellatwave))
        autofarmtab:Label("แจ้งเตือนผ่านดิสขอด: " .. tostring(getgenv().weburl))
        autofarmtab:Label("เล่นอัตโนมัต: " .. tostring(getgenv().AutoFarm))
        autofarmtab:Label("เริ่มเกมส์อัตโนมัต: " .. tostring(getgenv().autostart))
        autofarmtab:Label("ขายตัวทึ้งอัตโนมัต: " .. tostring(getgenv().autosell))
        autofarmtab:Label("อัพเกรดตัวละครอัตโนมัต: " .. tostring(getgenv().autoupgrade))
        autofarmtab:Label("ระดับความยาก: " .. tostring(getgenv().difficulty))
        autofarmtab:Label("โลกที่เลือก: " .. tostring(getgenv().world))
        autofarmtab:Label("เลเวล: " .. tostring(getgenv().level))
        autofarmtab:Label(" ")
        autofarmtab:Label(" ")
    end

    --------------------------------------------------
    -------------------- Misc Tab --------------------
    --------------------------------------------------

    if game.PlaceId == 8304191830 then
        local misc = serv:Channel("สุ่มกาชาเกลือ")

        misc:Toggle("สุมตัวโดยใช้ ตั๋ว อัตโนมัต", getgenv().autosummontickets, function(bool)
            getgenv().autosummontickets = bool
            while getgenv().autosummontickets do
                task.wait()
                local args = {
                    [1] = "dbz_fighter",
                    [2] = "ticket"
                }
                game:GetService("ReplicatedStorage").endpoints.client_to_server.buy_random_fighter:InvokeServer(unpack(
                    args))
            end
            updatejson()
        end)

        misc:Toggle("สุ่มตัวใช้ Gem ครั้งละ 1", getgenv().autosummongem, function(bool)
            getgenv().autosummongem = bool
            while getgenv().autosummongem do
                task.wait()
                local args = {
                    [1] = "dbz_fighter",
                    [2] = "gems"
                }

                game:GetService("ReplicatedStorage").endpoints.client_to_server.buy_random_fighter:InvokeServer(unpack(
                    args))
            end
            updatejson()
        end)

        misc:Toggle("สุมตัวโดยใช้ Gem ครั้งละ 10", getgenv().autosummongem10, function(bool)
            getgenv().autosummongem10 = bool
            while getgenv().autosummongem10 do
                task.wait()
                local args = {
                    [1] = "dbz_fighter",
                    [2] = "gems10"
                }

                game:GetService("ReplicatedStorage").endpoints.client_to_server.buy_random_fighter:InvokeServer(unpack(
                    args))

            end
            updatejson()
        end)
    end

 
   


end -- sex() function end

--------------------------------------------------
--------------------------------------------------

if isfile(savefilename) then -- checks if file exist or not
    sex()
else
    local xdata = {
        -- unitname = "name",
        -- unitid = "id",
        webhook = "",
        sellatwave = 0,
        autosell = false,
        autofarm = false,
        autostart = false,
        autoupgrade = false,
        difficulty = "Normal",
        world = "nil",
        level = "nil",
        door = "nil",
        xspawnUnitPos = {
            UP1 = {
                x = -2952.81689453125,
                y = 91.80620574951172,
                z = -707.9673461914062
            },

            UP2 = {
                x = -2952.81689453125,
                y = 91.80620574951172,
                z = -707.9673461914062
            },

            UP3 = {
                x = -2952.81689453125,
                y = 91.80620574951172,
                z = -707.9673461914062
            },

            UP4 = {
                x = -2952.81689453125,
                y = 91.80620574951172,
                z = -707.9673461914062
            }
        },
        xselectedUnits = {
            U1 = nil,
            U2 = nil,
            U3 = nil,
            U4 = nil
        }

    }
    local json = HttpService:JSONEncode(xdata)
    writefile(savefilename, json)

    sex()
end

--------------------------------------------------
--------------------------------------------------

------// Auto Farm \\------
coroutine.resume(coroutine.create(function()
    while task.wait() do
        local _wave = game:GetService("Workspace"):WaitForChild("_wave_num")
        
        if getgenv().AutoFarm and not getgenv().disableatuofarm then
            if game.PlaceId ~= 8304191830 then

                for i = 1, 4 do
                    local unitinfo = getgenv().SelectedUnits["U" .. i]
                    if unitinfo ~= nil then

                        local unitinfo_ = unitinfo:split(" #")
                        local pos = getgenv().SpawnUnitPos["UP" .. i]
                        local args = {
                            [1] = unitinfo_[2],
                            [2] = CFrame.new(Vector3.new(pos["x"], pos["y"], pos["z"]), Vector3.new(0, 0, -1))
                        }

                        game:GetService("ReplicatedStorage").endpoints.client_to_server.spawn_unit:InvokeServer(unpack(
                            args))

                    end
                end
            end
        end
    end
end))

------// Auto Leave \\------
coroutine.resume(coroutine.create(function()
    while task.wait(2) do
        if getgenv().AutoFarm then
            if  game.PlaceId ~= 8304191830 then

                function afc()
                    game.Players.LocalPlayer.PlayerGui.ResultsUI.Enabled = false
                    task.wait(1.5)

                    local VirtualInputManager = game:GetService("VirtualInputManager")
                    local X, Y = 0, 0
                    VirtualInputManager:SendMouseButtonEvent(X, Y, 0, true, game, 1)
                    VirtualInputManager:SendMouseButtonEvent(X, Y, 0, false, game, 1)
                    

                    if game:GetService("Workspace")["_DATA"].GameFinished.Value == true then
                        webhook()
                        task.wait(2)
                    warn("Teleporting Back To Lobby....")
                    game:GetService("ReplicatedStorage").endpoints.client_to_server.teleport_back_to_lobby:InvokeServer()
                    task.wait(15)
                    end
                end

                function clicknext()
                    if game.Players.LocalPlayer.PlayerGui.ResultsUI.Enabled == true then
                        for i,v in pairs(getconnections(game.Players.LocalPlayer.PlayerGui.ResultsUI.Holder.Buttons.Next.Activated)) do
                            v:Fire()
                        end
                    end
                end


                if game.Players.LocalPlayer.PlayerGui.ResultsUI.Enabled == true then
                    spawn(function()
                        clicknext()
                    end)
                    spawn(function()
                        afc()
                    end)
                end
            end
        end
    end
end))

------// Auto Upgrade \\------
coroutine.resume(coroutine.create(function()
    while task.wait() do
        if getgenv().autoupgrade then
            if game.PlaceId ~= 8304191830 then
                local max = 7
                repeat task.wait() until game:GetService("Workspace"):FindFirstChild("_UNITS")
                for i, v in ipairs(game:GetService("Workspace")["_UNITS"]:GetChildren()) do
                    repeat task.wait() until v:FindFirstChild("_stats")
                    if tostring(v["_stats"].player.Value) == game.Players.LocalPlayer.Name then
                        repeat task.wait() until v:FindFirstChild("_stats"):FindFirstChild("upgrade")

                        if v["_stats"].upgrade.Value == 0 or v["_stats"].upgrade.Value <= max then
                            game:GetService("ReplicatedStorage").endpoints.client_to_server.upgrade_unit_ingame:InvokeServer(
                                v)
                            --print(max, v["_stats"].upgrade.Value)
                        end
                    end
                end
            end
        end
    end
end))


------// Auto Sell \\------
coroutine.resume(coroutine.create(function()
    while task.wait() do
        local _wave = game:GetService("Workspace"):WaitForChild("_wave_num")
        if getgenv().autosell and tonumber(getgenv().sellatwave) <= _wave.Value then
            getgenv().disableatuofarm = true
            if game.PlaceId ~= 8304191830 then
                repeat task.wait() until game:GetService("Workspace"):FindFirstChild("_UNITS")
                for i, v in ipairs(game:GetService("Workspace")["_UNITS"]:GetChildren()) do
                    repeat
                        task.wait()
                    until v:FindFirstChild("_stats")
                    if tostring(v["_stats"].player.Value) == game.Players.LocalPlayer.Name then
                        repeat
                            task.wait()
                        until v:FindFirstChild("_stats"):FindFirstChild("upgrade")
            
                        game:GetService("ReplicatedStorage").endpoints.client_to_server.sell_unit_ingame:InvokeServer(v)
                    end
                end
            end
        end
    end
end))

------// Auto Start \\------
coroutine.resume(coroutine.create(function()
    while task.wait() do
        if getgenv().autostart and getgenv().AutoFarm then
            if game.PlaceId == 8304191830 then
                for i, v in pairs(game:GetService("Workspace")["_LOBBIES"].Story:GetDescendants()) do
                    if v.Name == "Owner" and v.Value == nil then
                        getgenv().door = v.Parent.Name
                        break
                    end
                end

                task.wait(0.1)

                local args = {
                    [1] = getgenv().door
                }
                game:GetService("ReplicatedStorage").endpoints.client_to_server.request_join_lobby:InvokeServer(unpack(
                    args))

                    task.wait(0.1)

                local args = {
                    [1] = getgenv().door, -- Lobby 
                    [2] = getgenv().level, -- World
                    [3] = true, -- Friends Only or not
                    [4] = getgenv().difficulty
                }
                game:GetService("ReplicatedStorage").endpoints.client_to_server.request_lock_level:InvokeServer(unpack(
                    args))

                    task.wait(0.1)

                local args = {
                    [1] = getgenv().door
                }

                game:GetService("ReplicatedStorage").endpoints.client_to_server.request_start_game:InvokeServer(unpack(
                    args))

            end
        end
    end
end))

while true do wait()
    pcall(function()
game.Players.LocalPlayer:Kick("ปิดอัพเดทโปรชั่วคราว")
end) end   
