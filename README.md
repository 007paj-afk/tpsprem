local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local Lighting = game:GetService("Lighting")
local LocalPlayer = Players.LocalPlayer

for i,b in pairs(workspace.FE.Actions:GetChildren()) do
    if b.Name == " " then
        b:Destroy()
    end
end

for i,b in pairs(LocalPlayer.Character:GetChildren()) do
    if b.Name == " " then
        b:Destroy()
    end
end

local a = workspace.FE.Actions
if a:FindFirstChild("KeepYourHeadUp_") then
    a.KeepYourHeadUp_:Destroy()
    local r = Instance.new("RemoteEvent")
    r.Name = "KeepYourHeadUp_"
    r.Parent = a
else
    LocalPlayer:Kick("Anti-Cheat Updated! Send a photo of this Message in our Discord Server so we can fix it.")
end

local function isWeirdName(name)
    return string.match(name, "^[a-zA-Z]+%-%d+%a*%-%d+%a*$") ~= nil
end

local function deleteWeirdRemoteEvents(parent)
    for _, child in pairs(parent:GetChildren()) do
        if child:IsA("RemoteEvent") and isWeirdName(child.Name) then
            child:Destroy()
        end
        deleteWeirdRemoteEvents(child)
    end
end

deleteWeirdRemoteEvents(game)

local WindUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/Footagesus/WindUI/main/dist/main.lua"))()

local Window = WindUI:CreateWindow({
    Title = "L1ght.dev | TPS STREET SOCCER | cracked by histo/w1nd |",
    Folder = "L1ghtScript",
    IconSize = 20,
    NewElements = true,
    Size = UDim2.fromOffset(580, 500),
    HideSearchBar = false,
    OpenButton = {
        Title = "L1ght",
        CornerRadius = UDim.new(1, 0),
        StrokeThickness = 3,
        Enabled = true,
        Draggable = true,
        OnlyMobile = false,
        Scale = 1,
        Color = ColorSequence.new(Color3.fromHex("#2ccb6f"), Color3.fromHex("#2ccb6f"))
    },
    Topbar = {
        Height = 44,
        ButtonsType = "Mac",
    },
})

do
    WindUI:AddTheme({
        Name = "L1ghtTheme",
        Accent = Color3.fromHex("#2ccb6f"),
        Dialog = Color3.fromHex("#121212"),
        Outline = Color3.fromHex("#2ccb6f"),
        Text = Color3.fromHex("#ffffff"),
        Placeholder = Color3.fromHex("#A0A0A0"),
        Button = Color3.fromHex("#1A1A1A"),
        Icon = Color3.fromHex("#2ccb6f"),
        WindowBackground = Color3.fromHex("#080808"),
        TopbarButtonIcon = Color3.fromHex("#ffffff"),
        TopbarTitle = Color3.fromHex("#ffffff"),
        TopbarAuthor = Color3.fromHex("#A0A0A0"),
        TopbarIcon = Color3.fromHex("#2ccb6f"),
        TabBackground = Color3.fromHex("#0f0f0f"),
        TabTitle = Color3.fromHex("#A0A0A0"),
        TabIcon = Color3.fromHex("#2ccb6f"),
        ElementBackground = Color3.fromHex("#151515"),
        ElementTitle = Color3.fromHex("#ffffff"),
        ElementDesc = Color3.fromHex("#AAAAAA"),
        ElementIcon = Color3.fromHex("#2ccb6f"),
    })
    WindUI:SetTheme("L1ghtTheme")
end

local Main = Window:Section({ Title = "Main" })
local ReachTab = Main:Tab({ Title = "Reach", Icon = "user" })
local MossingTab = Main:Tab({ Title = "Mossing", Icon = "headphones" })
local ReactTab = Main:Tab({ Title = "React", Icon = "zap" })

local Misc = Window:Section({ Title = "Miscellaneous" })
local PlayerTab = Misc:Tab({ Title = "Player", Icon = "monitor" })
local HelpersTab = Misc:Tab({ Title = "Helpers", Icon = "cloud" })
local BallModTab = Misc:Tab({ Title = "Ball Modifcation", Icon = "banknote" })
local AimbotTab = Misc:Tab({ Title = "Aimbot", Icon = "crosshair" })
local GamepassTab = Misc:Tab({ Title = "Free Gamepass", Icon = "gamepad-2" })
local TeleportTab = Misc:Tab({ Title = "Teleport", Icon = "telescope" })
local SkyTab = Misc:Tab({ Title = "Sky Changer", Icon = "sun" }) 

local reachEnabled = false
local reachDistance = 1
local reachConnection

ReachTab:Section({ Title = "Reach Legs (Normal) (Firetouchinterest)" })

ReachTab:Toggle({
    Title = "Enable / Disable Reach (Firetouchinterest)",
    Callback = function(Value)
        reachEnabled = Value
        if not Value and reachConnection then
            reachConnection:Disconnect()
            reachConnection = nil
        end

        if Value then
            if reachConnection then reachConnection:Disconnect() end
            reachConnection = RunService.RenderStepped:Connect(function()
                local player = LocalPlayer
                local character = player and player.Character
                local rootPart = character and character:FindFirstChild("HumanoidRootPart")
                local humanoid = character and character:FindFirstChild("Humanoid")

                if not (character and rootPart and humanoid) then return end
                local tps = Workspace:FindFirstChild("TPSSystem") and Workspace.TPSSystem:FindFirstChild("TPS")
                if not tps then return end

                local distance = (rootPart.Position - tps.Position).Magnitude
                if distance <= reachDistance then
                    local preferredFoot = Lighting:FindFirstChild(player.Name) and Lighting[player.Name]:FindFirstChild("PreferredFoot")
                    if preferredFoot then
                        local limbName = (humanoid.RigType == Enum.HumanoidRigType.R6)
                            and ((preferredFoot.Value == 1) and "Right Leg" or "Left Leg")
                            or ((preferredFoot.Value == 1) and "RightLowerLeg" or "LeftLowerLeg")

                        local limb = character:FindFirstChild(limbName)
                        if limb then
                            firetouchinterest(limb, tps, 0)
                            firetouchinterest(limb, tps, 1)
                        end
                    end
                end
            end)
        end
    end
})

ReachTab:Slider({
    Title = "Reach Size",
    Desc = "The Size Of The Reach",
    Value = { Min = 1, Max = 15, Default = 1 },
    Callback = function(val)
        reachDistance = tonumber(val)
    end
})

ReachTab:Section({ Title = "Reach Legs (Editing Size)" })

ReachTab:Input({
    Title = "Legs Size (R6)",
    Desc = "Minimum Size is 1",
    Value = "1",
    Callback = function(Value) 
        local v = tonumber(Value) or 1
        if LocalPlayer.Character then
            if LocalPlayer.Character:FindFirstChild("Right Leg") then
                LocalPlayer.Character["Right Leg"].Size = Vector3.new(v, 2, v)
                LocalPlayer.Character["Left Leg"].Size = Vector3.new(v, 2, v)
                LocalPlayer.Character["Right Leg"].CanCollide = false
                LocalPlayer.Character["Left Leg"].CanCollide = false
            end
            if LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                LocalPlayer.Character.HumanoidRootPart.Size = Vector3.new(v,2,v)
                LocalPlayer.Character.HumanoidRootPart.CanCollide = false
            end
        end
    end
})

ReachTab:Input({
    Title = "Legs Size (R15)",
    Desc = "Minimum Size is 1",
    Value = "1",
    Callback = function(Value) 
        local v = tonumber(Value) or 1
        if LocalPlayer.Character then
            if LocalPlayer.Character:FindFirstChild("RightLowerLeg") then
                LocalPlayer.Character["RightLowerLeg"].Size = Vector3.new(v, 2, v)
                LocalPlayer.Character["LeftLowerLeg"].Size = Vector3.new(v, 2, v)
                LocalPlayer.Character["RightLowerLeg"].CanCollide = false
                LocalPlayer.Character["LeftLowerLeg"].CanCollide = false
            end
            if LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                LocalPlayer.Character.HumanoidRootPart.Size = Vector3.new(v,2,v)
                LocalPlayer.Character.HumanoidRootPart.CanCollide = false
            end
        end
    end
})

ReachTab:Button({
    Title = "Fake legs (Appear Normal)",
    Callback = function()
        local player = LocalPlayer
        local character = player.Character or player.CharacterAdded:Wait()
        local humanoid = character:WaitForChild("Humanoid")

        if humanoid.RigType == Enum.HumanoidRigType.R6 then
            character["Right Leg"].Transparency = 1
            character["Left Leg"].Transparency = 1

            character["Left Leg"].Massless = true
            local LeftLegM = Instance.new("Part", character)
            LeftLegM.Name = "Left Leg Fake"
            LeftLegM.CanCollide = false
            LeftLegM.Color = character["Left Leg"].Color
            LeftLegM.Size = Vector3.new(1, 2, 1)
            LeftLegM.Position = character["Left Leg"].Position
            
            local MotorHip = Instance.new("Motor6D", character.Torso)
            MotorHip.Part0 = character.Torso
            MotorHip.Part1 = LeftLegM
            MotorHip.C0 = CFrame.new(-1, -1, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0)
            MotorHip.C1 = CFrame.new(-0.5, 1, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0)

            character["Right Leg"].Massless = true
            local RightLegM = Instance.new("Part", character)
            RightLegM.Name = "Right Leg Fake"
            RightLegM.CanCollide = false
            RightLegM.Color = character["Right Leg"].Color
            RightLegM.Size = Vector3.new(1, 2, 1)
            RightLegM.Position = character["Right Leg"].Position

            local MotorHip2 = Instance.new("Motor6D", character.Torso)
            MotorHip2.Part0 = character.Torso
            MotorHip2.Part1 = RightLegM
            MotorHip2.C0 = CFrame.new(1, -1, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0)
            MotorHip2.C1 = CFrame.new(0.5, 1, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0)
        end
    end
})

local headReachEnabled = false
local headReachSize = Vector3.new(1, 1.5, 1)
local headTransparency = 0.5
local headOffset = Vector3.new(0, 0, 0)
local headBoxPart
local headConnection

local function updateHeadBox()
    if headBoxPart then headBoxPart:Destroy() end
    headBoxPart = Instance.new("Part")
    headBoxPart.Size = headReachSize
    headBoxPart.Transparency = headTransparency
    headBoxPart.Anchored = true
    headBoxPart.CanCollide = false
    headBoxPart.Color = Color3.fromRGB(44, 203, 111)
    headBoxPart.Material = Enum.Material.Neon
    headBoxPart.Name = "HeadReachBox"
    headBoxPart.Parent = Workspace
end

local function startHeadReach()
    if not headReachEnabled then return end
    if headConnection then headConnection:Disconnect() end
    updateHeadBox()
    headConnection = RunService.RenderStepped:Connect(function()
        local character = LocalPlayer.Character
        if not character then return end
        local head = character:FindFirstChild("Head")
        local tps = Workspace:FindFirstChild("TPSSystem") and Workspace.TPSSystem:FindFirstChild("TPS")
        if not (head and tps) then return end
        
        headBoxPart.CFrame = head.CFrame * CFrame.new(headOffset)
        
        local relative = headBoxPart.CFrame:PointToObjectSpace(tps.Position)
        if math.abs(relative.X) <= headBoxPart.Size.X / 2 
            and math.abs(relative.Y) <= headBoxPart.Size.Y / 2 
            and math.abs(relative.Z) <= headBoxPart.Size.Z / 2 then
            firetouchinterest(head, tps, 0)
            firetouchinterest(head, tps, 1)
        end
    end)
end

MossingTab:Toggle({
    Title = "Enable / Disable Moss & Head Reach",
    Callback = function(state)
        headReachEnabled = state
        if state then startHeadReach() else 
            if headConnection then headConnection:Disconnect() end
            if headBoxPart then headBoxPart:Destroy() end
        end
    end
})

MossingTab:Slider({
    Title = "Reach X",
    Value = { Min = 0, Max = 50, Default = 1 },
    Callback = function(val)
        headReachSize = Vector3.new(val, headReachSize.Y, headReachSize.Z)
        if headReachEnabled then updateHeadBox() end
    end
})

MossingTab:Slider({
    Title = "Reach Y",
    Value = { Min = 0, Max = 50, Default = 1.5 },
    Callback = function(val)
        headReachSize = Vector3.new(headReachSize.X, val, headReachSize.Z)
        headOffset = Vector3.new(headOffset.X, val / 2.5, headOffset.Z)
        if headReachEnabled then updateHeadBox() end
    end
})

MossingTab:Slider({
    Title = "Reach Z",
    Value = { Min = 0, Max = 50, Default = 1 },
    Callback = function(val)
        headReachSize = Vector3.new(headReachSize.X, headReachSize.Y, val)
        if headReachEnabled then updateHeadBox() end
    end
})

MossingTab:Toggle({
    Title = "Appear Normal",
    Callback = function(v)
        headTransparency = v and 1 or 0.5
        if headReachEnabled and headBoxPart then
            headBoxPart.Transparency = headTransparency
        end
    end
})

ReactTab:Section({ Title = "Ball Reactions" })

ReactTab:Button({
    Title = "Better React",
    Callback = function()
        if Workspace.TPSSystem:FindFirstChild("TPS") then
            Workspace.TPSSystem.TPS.Velocity = Vector3.new(100, 100, 100)
        end
    end
})

ReactTab:Button({
    Title = "Alz React",
    Callback = function()
        if Workspace.TPSSystem:FindFirstChild("TPS") then
            Workspace.TPSSystem.TPS.Velocity = Vector3.new(100, 100, 100)
        end
    end
})

ReactTab:Button({
    Title = "Foxtede React",
    Callback = function()
        if Workspace.TPSSystem:FindFirstChild("TPS") then
            Workspace.TPSSystem.TPS.Velocity = Vector3.new(110, 110, 110)
        end
    end
})

ReactTab:Button({
    Title = "Goalkeeper React",
    Callback = function()
        local gkActions = {"SaveRA", "SaveLA", "SaveRL", "SaveLL", "SaveT", "Tackle", "Header"}
        for _, action in ipairs(gkActions) do
            local meta = getrawmetatable(game)
            local oldNamecall = meta.namecall
            setreadonly(meta, false)
            meta.namecall = newcclosure(function(self, ...)
                local method = tostring(getnamecallmethod())
                if method == "FireServer" and tostring(self) == action then
                    local args = {...}
                    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
                        args[2] = LocalPlayer.Character.Humanoid.LLCL
                        return oldNamecall(self, unpack(args))
                    end
                end
                return oldNamecall(self, ...)
            end)
            setreadonly(meta, true)
        end
    end
})

PlayerTab:Section({ Title = "Walkspeed" })
local wsEnabled, currentSpeed = false, 23
PlayerTab:Toggle({
    Title = "Enable / Disable Walkspeed",
    Callback = function(v)
        wsEnabled = v
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = v and currentSpeed or 23
        end
    end
})

PlayerTab:Slider({
    Title = "Walkspeed Size",
    Value = { Min = 23, Max = 75, Default = 23 },
    Callback = function(v)
        currentSpeed = v
        if wsEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = v
        end
    end
})

PlayerTab:Section({ Title = "Jump Power" })
local jumpEnabled, currentJump = false, 50
PlayerTab:Toggle({
    Title = "Enable / Disable Jump Power",
    Callback = function(v)
        jumpEnabled = v
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.JumpPower = v and currentJump or 50
        end
    end
})

PlayerTab:Slider({
    Title = "Jump Power",
    Value = { Min = 50, Max = 120, Default = 50 },
    Callback = function(v)
        currentJump = v
        if jumpEnabled and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.JumpPower = v
        end
    end
})

PlayerTab:Section({ Title = "Avatar Stolen" })
local disguiseEnabled = false
local disguiseUser = ""
PlayerTab:Toggle({
    Title = "Enable / Disable Avatar Stolen",
    Callback = function(state)
        disguiseEnabled = state
        if state and disguiseUser ~= "" and LocalPlayer.Character then
            local success, id = pcall(function() return Players:GetUserIdFromNameAsync(disguiseUser) end)
            if success then
                local desc = Players:GetHumanoidDescriptionFromUserId(id)
                LocalPlayer.Character.Humanoid:ApplyDescriptionClientServer(desc)
            end
        elseif not state and LocalPlayer.Character then
            local desc = Players:GetHumanoidDescriptionAsync(LocalPlayer.UserId)
            LocalPlayer.Character.Humanoid:ApplyDescriptionClientServer(desc)
        end
    end
})

PlayerTab:Input({
    Title = "Avatar Stolen",
    Placeholder = "Enter user...",
    Callback = function(input)
        disguiseUser = input
    end
})

HelpersTab:Section({ Title = "Helpers" })

HelpersTab:Toggle({
    Title = "Enable / Disable ZZZZ Helper",
    Callback = function(state)
        if state then
            local part = Instance.new("Part")
            part.Name = "TPS1"
            part.Size = Vector3.new(9, 0.1, 9)
            part.Anchored = true
            part.BrickColor = BrickColor.new("Bright red")
            part.Transparency = 1 
            part.Parent = Workspace
            
            RunService.RenderStepped:Connect(function()
                local tpsTarget = Workspace:FindFirstChild("TPSSystem") and Workspace.TPSSystem:FindFirstChild("TPS")
                if tpsTarget and part.Parent then
                    part.Position = tpsTarget.Position - Vector3.new(0, 1, 0)
                end
            end)
        else
            if Workspace:FindFirstChild("TPS1") then Workspace.TPS1:Destroy() end
        end
    end
})

HelpersTab:Section({ Title = "Air Dribble Helper" })

HelpersTab:Toggle({
    Title = "Enable / Disable Air Dribble Helper",
    Callback = function(state)
        if not state and Workspace:FindFirstChild("TPS_Air") then
            Workspace.TPS_Air:Destroy()
        end
    end
})

HelpersTab:Slider({
    Title = "Air Dribble Helper Size",
    Value = { Min = 1, Max = 15, Default = 1 },
    Callback = function(val)
        local part = Workspace:FindFirstChild("TPS_Air") or Instance.new("Part")
        part.Name = "TPS_Air"
        part.Size = Vector3.new(val, 0.1, val)
        part.Anchored = true
        part.BrickColor = BrickColor.new("Bright red")
        part.Transparency = 1
        part.Parent = Workspace
        
        RunService.RenderStepped:Connect(function()
            local tpsTarget = Workspace:FindFirstChild("TPSSystem") and Workspace.TPSSystem:FindFirstChild("TPS")
            if tpsTarget and part.Parent then
                part.Position = tpsTarget.Position - Vector3.new(0, 1, 0)
            end
        end)
    end
})

HelpersTab:Section({ Title = "Inf Dribble Helper" })

local followBall = false
local toggleEnabled = false

HelpersTab:Toggle({
    Title = "Enable / Disable Inf Dribble Helper [PC]",
    Desc = "Toggle it by pressing (B)",
    Callback = function(state)
        toggleEnabled = state
        if not state then followBall = false end
    end
})

UserInputService.InputBegan:Connect(function(input, gp)
    if input.KeyCode == Enum.KeyCode.B and not gp and toggleEnabled then
        followBall = not followBall
    end
end)

RunService.RenderStepped:Connect(function()
    if followBall and toggleEnabled then
        local ball = Workspace.TPSSystem.TPS
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid:MoveTo(ball.Position)
        end
    end
end)

BallModTab:Section({ Title = "Ball Modifications" })

BallModTab:Input({
    Title = "Ball Size",
    Value = "2.6",
    Callback = function(val)
        if Workspace.TPSSystem:FindFirstChild("TPS") then
            Workspace.TPSSystem.TPS.Size = Vector3.new(tonumber(val), tonumber(val), tonumber(val))
        end
    end
})

BallModTab:Button({
    Title = "Anti-Ball Fling",
    Callback = function()
        local char = LocalPlayer.Character
        if char then
            for _, part in pairs(char:GetDescendants()) do
                if part:IsA("BasePart") and (part.Name == "Right Leg" or part.Name == "Right Arm" or part.Name == "Left Arm" or part.Name == "Torso") then
                    part.CanCollide = false
                end
            end
        end
    end
})

BallModTab:Toggle({
    Title = "Enable / Disable Ball Collision",
    Callback = function(state)
        if state then
            local TPS = Workspace.TPSSystem.TPS
            local follower = Instance.new("Part", Workspace)
            follower.Name = "FollowerPart"
            follower.Shape = Enum.PartType.Ball
            follower.Anchored = true
            follower.CanCollide = true
            follower.Transparency = 1
            RunService.Heartbeat:Connect(function()
                if TPS then
                    follower.Size = TPS.Size
                    follower.CFrame = TPS.CFrame
                end
            end)
        else
            if Workspace:FindFirstChild("FollowerPart") then Workspace.FollowerPart:Destroy() end
        end
    end
})

BallModTab:Dropdown({
    Title = "Ball Texture",
    Values = {"Default", "Addidas Brazuca", "Addidas Jabulani"},
    Callback = function(val)
        local ball = Workspace.TPSSystem.TPS
        if val == "Default" then
            ball.Decal1.Texture = "rbxassetid://1731401359"
            ball.Decal2.Texture = "rbxassetid://1731401359"
        elseif val == "Addidas Brazuca" then
            ball.Decal1.Texture = "http://www.roblox.com/asset/?id=137668136"
            ball.Decal2.Texture = "http://www.roblox.com/asset/?id=137668129"
        end
    end
})

local isAimbotEnabled = false
local aimbotTargetPos = Vector3.new(0, 14, 157)
local laser = Instance.new("Part")
laser.Name = "L1ght.dev aimbot"
laser.Anchored = true
laser.CanCollide = false
laser.Material = Enum.Material.Neon
laser.Color = Color3.fromRGB(255, 0, 0)
laser.Transparency = 1 
laser.Size = Vector3.new(0.05, 0.05, 1) 
laser.Parent = Workspace

local function toggleAimbot(state)
    isAimbotEnabled = state
    laser.Transparency = isAimbotEnabled and 0.4 or 1
end

RunService:BindToRenderStep("L1ghtAimbotLoop", Enum.RenderPriority.Camera.Value + 1, function()
    local char = LocalPlayer.Character
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    local torso = char and (char:FindFirstChild("UpperTorso") or char:FindFirstChild("Torso"))
    
    if isAimbotEnabled and hrp and torso then
        local hrpPos = hrp.Position
        local lookTarget = Vector3.new(aimbotTargetPos.X, hrpPos.Y, aimbotTargetPos.Z)
        hrp.CFrame = CFrame.lookAt(hrpPos, lookTarget)
        
        local startPos = torso.Position + Vector3.new(0, 0.8, 0) 
        local distance = (aimbotTargetPos - startPos).Magnitude
        laser.Size = Vector3.new(0.05, 0.05, distance)
        laser.CFrame = CFrame.lookAt(startPos, aimbotTargetPos) * CFrame.new(0, 0, -distance/2)
    end
end)

AimbotTab:Section({ Title = "Aimbot Settings" })

local AimbotToggle = AimbotTab:Toggle({
    Title = "Enable / Disable Aimbot",
    Callback = function(state)
        toggleAimbot(state)
    end
})

AimbotTab:Keybind({
    Title = "Aimbot Keybind",
    Default = Enum.KeyCode.R,
    Callback = function()
        local newState = not isAimbotEnabled
        AimbotToggle:Set(newState) 
    end
})

GamepassTab:Section({ Title = "Free Gamepasses" })

GamepassTab:Toggle({
    Title = "More Curve",
    Callback = function(state)
        local gui = LocalPlayer.PlayerGui.Start.GamePassMenu.Items.WoodenFloor
        gui.Tick.Visible = state
        gui.WoodenFloor.Style = state and "RobloxRoundButton" or "RobloxRoundDefaultButton"
        if state then
             LocalPlayer.Character.Humanoid.AnimationPlayed:Connect(function(AnimationTrack)
                if table.find({"OldMKickL","OldMKick","MKick"}, AnimationTrack.Name) then
                    local event = Workspace.FE.Actions["KickC1"]
                    event:FireServer(Workspace.TPSSystem.TPS, LocalPlayer.Character.HumanoidRootPart)
                end
            end)
        end
    end
})

GamepassTab:Toggle({
    Title = "Blue Flame",
    Callback = function(state)
        if state then
            local start = LocalPlayer.PlayerGui:WaitForChild("Start")
            start.PowerShot.Flame.Image = "rbxassetid://15316155280"
            start.PowerShot.Flame.Flame.Image = "rbxassetid://15316155280"
            LocalPlayer.Backpack.FValue.Value = 2
            start.GamePassMenu.Items.BlueFlame.Tick.Visible = true
        end
    end
})

GamepassTab:Toggle({
    Title = "Unlock Cooldown",
    Callback = function(state)
        if state then
            LocalPlayer.PlayerGui:WaitForChild("Start").PowerShot.PowerValue.Value = 30
            LocalPlayer.PlayerGui.Start.GamePassMenu.Items.Cooldown.Tick.Visible = true
        end
    end
})

GamepassTab:Toggle({
    Title = "Powerful Tackle",
    Callback = function(state)
        local gui = LocalPlayer.PlayerGui.Start.GamePassMenu.Items.RandomWeather
        gui.Tick.Visible = state
        gui.RandomWeather.Style = state and "RobloxRoundButton" or "RobloxRoundDefaultButton"
        LocalPlayer.Backpack.TackleGamePass.Value = state
    end
})

TeleportTab:Section({ Title = "Teleporting" })

local tpTarget = nil
TeleportTab:Input({
    Title = "Target Player Username",
    Placeholder = "Enter Text...",
    Callback = function(txt)
        tpTarget = Players:FindFirstChild(txt)
    end
})

TeleportTab:Button({
    Title = "Teleport To Player",
    Callback = function()
        if tpTarget and LocalPlayer.Character and tpTarget.Character then
            LocalPlayer.Character.HumanoidRootPart.CFrame = tpTarget.Character.HumanoidRootPart.CFrame
        end
    end
})

TeleportTab:Button({
    Title = "Teleport Above Player",
    Callback = function()
        if tpTarget and LocalPlayer.Character and tpTarget.Character then
            LocalPlayer.Character.HumanoidRootPart.CFrame = tpTarget.Character.HumanoidRootPart.CFrame + Vector3.new(0, 10, 0)
        end
    end
})

TeleportTab:Button({
    Title = "Teleport Under Player",
    Callback = function()
        if tpTarget and LocalPlayer.Character and tpTarget.Character then
            LocalPlayer.Character.HumanoidRootPart.CFrame = tpTarget.Character.HumanoidRootPart.CFrame + Vector3.new(0, -5, 0)
        end
    end
})

TeleportTab:Button({
    Title = "Teleport To Random Player",
    Callback = function()
        local list = Players:GetPlayers()
        local randomP = list[math.random(1, #list)]
        if randomP ~= LocalPlayer and randomP.Character then
            LocalPlayer.Character.HumanoidRootPart.CFrame = randomP.Character.HumanoidRootPart.CFrame + Vector3.new(0,3,0)
        end
    end
})

TeleportTab:Button({
    Title = "Teleport To Red Goal",
    Callback = function()
        if Workspace:FindFirstChild("RedGoal") then
            LocalPlayer.Character.HumanoidRootPart.CFrame = Workspace.RedGoal.Part.CFrame + Vector3.new(0,3,0)
        end
    end
})

TeleportTab:Button({
    Title = "Teleport To Blue Goal",
    Callback = function()
        if Workspace:FindFirstChild("BlueGoal") then
            LocalPlayer.Character.HumanoidRootPart.CFrame = Workspace.BlueGoal.Part.CFrame + Vector3.new(0,3,0)
        end
    end
})

local function SetSky(data, stars)
    if Lighting:FindFirstChildOfClass("Sky") then
        Lighting:FindFirstChildOfClass("Sky"):Destroy()
    end
    local sky = Instance.new("Sky")
    sky.SkyboxBk = data.Bk
    sky.SkyboxDn = data.Dn
    sky.SkyboxFt = data.Ft
    sky.SkyboxLf = data.Lf
    sky.SkyboxRt = data.Rt
    sky.SkyboxUp = data.Up
    sky.StarCount = stars or 3000
    sky.Parent = Lighting
end

SkyTab:Section({ Title = "Sky Presets" })

SkyTab:Button({
    Title = "Night Sky",
    Callback = function()
        SetSky({
            Bk="http://www.roblox.com/asset/?id=12064107",
            Dn="http://www.roblox.com/asset/?id=12064152",
            Ft="http://www.roblox.com/asset/?id=12064121",
            Lf="http://www.roblox.com/asset/?id=12063984",
            Rt="http://www.roblox.com/asset/?id=12064115",
            Up="http://www.roblox.com/asset/?id=12064131",
        }, 0)
    end
})

SkyTab:Button({
    Title = "Purple Sky",
    Callback = function()
        SetSky({
            Bk="http://www.roblox.com/asset/?id=570557514",
            Dn="http://www.roblox.com/asset/?id=570557775",
            Ft="http://www.roblox.com/asset/?id=570557559",
            Lf="http://www.roblox.com/asset/?id=570557620",
            Rt="http://www.roblox.com/asset/?id=570557672",
            Up="http://www.roblox.com/asset/?id=570557727",
        })
    end
})

WindUI:Notify({
    Title = "L1ght.dev",
    Desc = "Script Loaded - Full Feature Set",
    Icon = "check",
    Duration = 3
})
