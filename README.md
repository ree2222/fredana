
        
        
            if getgenv().biosourceloaded == true then
            if getgenv().biosource.Options.Notifications == true then
            
            game.StarterGui:SetCore(
                                "SendNotification",
                                {
                                    Title = "biosource",
                                    Text = "Script Already Loaded Copied Settings.",
                                    Duration = 1.5
                                }
                            )
                        end
                return
            end
            getgenv().biosourceloaded = true
            
            if getgenv().biosource.Options.Notifications == true then
            wait(0.5)
            game.StarterGui:SetCore(
            "SendNotification",
            {
                Title = "biosource",
                Text = "welcome.", 
                Duration = 1.5 
            }
            )
            end
        
        
            local Prey
            local Plr
        
            local Players, Client, Mouse, RS, Camera =
            game:GetService("Players"),
            game:GetService("Players").LocalPlayer,
            game:GetService("Players").LocalPlayer:GetMouse(),
            game:GetService("RunService"),
            game:GetService("Workspace").CurrentCamera
        
            local silentcircle = Drawing.new("Circle")
            local tracercircle = Drawing.new("Circle")
           
            silentcircle.Transparency = getgenv().biosource.Fov.Silent.Transparency
            silentcircle.Thickness = getgenv().biosource.Fov.Silent.Thickness
            silentcircle.Color = getgenv().biosource.Fov.Silent.Color
            silentcircle.Filled = getgenv().biosource.Fov.Silent.Filled
            tracercircle.Transparency = getgenv().biosource.Fov.Camlock.Transparency
            tracercircle.Thickness = getgenv().biosource.Fov.Camlock.Thickness
            tracercircle.Color = getgenv().biosource.Fov.Camlock.Color
            tracercircle.Filled = getgenv().biosource.Fov.Camlock.Filled
        
            local UpdateFOV = function ()
            if (not silentcircle and not tracercircle) then
                return silentcircle and tracercircle
            end
            tracercircle.Visible  = getgenv().biosource.Fov.Camlock.Visible
            tracercircle.Radius   = getgenv().biosource.Fov.Camlock.Size * 2
            tracercircle.Filled = getgenv().biosource.Fov.Camlock.Filled
            tracercircle.Color = getgenv().biosource.Fov.Camlock.Color
            tracercircle.Transparency = getgenv().biosource.Fov.Camlock.Transparency
            tracercircle.Thickness = getgenv().biosource.Fov.Camlock.Thickness
            tracercircle.Position = Vector2.new(Mouse.X, Mouse.Y + (game:GetService("GuiService"):GetGuiInset().Y))
            
            silentcircle.Visible  = getgenv().biosource.Fov.Silent.Visible
            silentcircle.Radius   = getgenv().biosource.Fov.Silent.Size * 2
            silentcircle.Filled = getgenv().biosource.Fov.Silent.Filled
            silentcircle.Color = getgenv().biosource.Fov.Silent.Color
            silentcircle.Transparency = getgenv().biosource.Fov.Silent.Transparency
            silentcircle.Thickness = getgenv().biosource.Fov.Silent.Thickness
            silentcircle.Position = Vector2.new(Mouse.X, Mouse.Y + (game:GetService("GuiService"):GetGuiInset().Y))
            return silentcircle and tracercircle
            end
        
            RS.Heartbeat:Connect(UpdateFOV)
        
        local WallCheck = function(destination, ignore)
            local Origin    = Camera.CFrame.p
            local CheckRay  = Ray.new(Origin, destination - Origin)
            local Hit = game.workspace:FindPartOnRayWithIgnoreList(CheckRay, ignore)
            return Hit == nil
        end
        
        
        local WTS = function (Object)
            local ObjectVector = Camera:WorldToScreenPoint(Object.Position)
            return Vector2.new(ObjectVector.X, ObjectVector.Y)
        end
        
        local IsOnScreen = function (Object)
            local IsOnScreen = Camera:WorldToScreenPoint(Object.Position)
            return IsOnScreen
        end
        
        local FilterObjs = function (Object)
            if string.find(Object.Name, "Gun") then
                return
            end
            if table.find({"Part", "MeshPart", "BasePart"}, Object.ClassName) then
                return true
            end
        end
        
        local ClosestPlrFromMouse = function()
            local Target, Closest = nil, 1/0
            
            for _ ,v in pairs(Players:GetPlayers()) do
                if getgenv().biosource.Checks.Wall then
                    if (v.Character and v ~= Client and v.Character:FindFirstChild("HumanoidRootPart")) then
                        wait(0.01)
                        local Position, OnScreen = Camera:WorldToScreenPoint(v.Character.HumanoidRootPart.Position)
                        local Distance = (Vector2.new(Position.X, Position.Y) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude
            
                        if (silentcircle.Radius * 1.27 > Distance and Distance < Closest and OnScreen) and WallCheck(v.Character.HumanoidRootPart.Position, {Client, v.Character}) then
                            Closest = Distance
                            Target = v
                
        
                        end
                    end
                else
                    if (v.Character and v ~= Client and v.Character:FindFirstChild("HumanoidRootPart")) then
                        local Position, OnScreen = Camera:WorldToScreenPoint(v.Character.HumanoidRootPart.Position)
                        local Distance = (Vector2.new(Position.X, Position.Y) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude
            
                        if (silentcircle.Radius * 1.27 > Distance and Distance < Closest and OnScreen) then
                            Closest = Distance
                            Target = v
                        end
                    end
                end
            end
            return Target
        end
        
        local ClosestPlrFromMouse2 = function()
            local Target, Closest = nil, tracercircle.Radius * 1.27
            
            for _ ,v in pairs(Players:GetPlayers()) do
                if (v.Character and v ~= Client and v.Character:FindFirstChild("HumanoidRootPart")) then
                    if getgenv().biosource.Checks.Wall then
                        local Position, OnScreen = Camera:WorldToScreenPoint(v.Character.HumanoidRootPart.Position)
                        local Distance = (Vector2.new(Position.X, Position.Y) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude
                
                        if (Distance < Closest and OnScreen) and WallCheck(v.Character.HumanoidRootPart.Position, {Client, v.Character}) then
                            Closest = Distance
                            Target = v
                        end
                        else
                            local Position, OnScreen = Camera:WorldToScreenPoint(v.Character.HumanoidRootPart.Position)
                            local Distance = (Vector2.new(Position.X, Position.Y) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude
                
                            if (Distance < Closest and OnScreen) then
                                Closest = Distance
                                Target = v
                            end
                        end
                    end
                end
            return Target
        end
        
        local GetClosestBodyPart = function (character)
            local ClosestDistance = 1/0
            local BodyPart = nil
            
            if (character and character:GetChildren()) then
                for _,  x in next, character:GetChildren() do
                    if FilterObjs(x) and IsOnScreen(x) then
                        local Distance = (WTS(x) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude
                        if (silentcircle.Radius * 1.27 > Distance and Distance < ClosestDistance) then
                            ClosestDistance = Distance
                            BodyPart = x
                        end
                    end
                end
            end
            return BodyPart
        end
        
        local GetClosestBodyPartV2 = function (character)
            local ClosestDistance = 1/0
            local BodyPart = nil
            
            if (character and character:GetChildren()) then
                for _,  x in next, character:GetChildren() do
                    if FilterObjs(x) and IsOnScreen(x) then
                        local Distance = (WTS(x) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude
                        if (Distance < ClosestDistance) then
                            ClosestDistance = Distance
                            BodyPart = x
                        end
                    end
                end
            end
            return BodyPart
        end
        
        Mouse.KeyDown:Connect(function(Key)
            local Keybind = getgenv().biosource.Camlock.Keybind:lower()
            if (Key == Keybind) then
                if getgenv().biosource.Camlock.Enabled == true then
                    IsTargetting = not IsTargetting
                    if IsTargetting then
                        Plr = ClosestPlrFromMouse2()
                    else
                        if Plr ~= nil then
                            Plr = nil
                            IsTargetting = false
                        end
                    end
                end
            end
        end)
        
        Mouse.KeyDown:Connect(function(Key)
            local Keybind = getgenv().biosource.Silent.Keybind:lower()
            if (Key == Keybind) and getgenv().biosource.Silent.UseKeybind == true then
                    if getgenv().biosource.Silent.Enabled == true then
                        getgenv().biosource.Silent.Enabled = false
                        if getgenv().biosource.Options.Notifications == true then
                            
        game.StarterGui:SetCore(
            "SendNotification",
            {
                Title = "biosource", 
                Text = "Silent Disabled", 
                Duration = 1.5 
            }
        )
                     end   
                    else
                        getgenv().biosource.Silent.Enabled = true
                        if getgenv().biosource.Options.Notifications == true then
                            
        game.StarterGui:SetCore(
            "SendNotification",
            {
                Title = "biosource", 
                Text = "Silent Enabled", 
                Duration = 1.5 
            }
        )
        end
        end
        end
        end)
        
        local grmt = getrawmetatable(game)
        local backupindex = grmt.__index
        setreadonly(grmt, false)
        
        grmt.__index = newcclosure(function(self, v)
            if (getgenv().biosource.Silent.Enabled and Mouse and tostring(v) == "Hit") then
                if Prey and Prey.Character then
                    if getgenv().biosource.Silent.Predict then
                        local endpoint = game.Players[tostring(Prey)].Character[getgenv().biosource.Silent.Aimpart].CFrame + (
                            game.Players[tostring(Prey)].Character[getgenv().biosource.Silent.Aimpart].Velocity * getgenv().biosource.Silent.Prediction
                        )
                        return (tostring(v) == "Hit" and endpoint)
                    else
                        local endpoint = game.Players[tostring(Prey)].Character[getgenv().biosource.Silent.Aimpart].CFrame
                        return (tostring(v) == "Hit" and endpoint)
                    end
                end
            end
            return backupindex(self, v)
        end)
        
        
        
        RS.Heartbeat:Connect(function()
            if getgenv().biosource.Silent.Enabled then
                if Prey and Prey.Character and Prey.Character:WaitForChild(getgenv().biosource.Silent.Aimpart) then
                    if getgenv().biosource.Resolver.Enabled == true and Prey.Character:WaitForChild("HumanoidRootPart").Velocity.magnitude > 70 then            
                        pcall(function()
                            local TargetVel = Prey.Character[getgenv().biosource.Silent.Aimpart]
                            TargetVel.Velocity = Vector3.new(0, 0, 0)
                            TargetVel.AssemblyLinearVelocity = Vector3.new(0, 0, 0)
                    
                        end)
                    end
                    if getgenv().biosource.Checks.AntiGroundShots == true and Prey.Character:FindFirstChild("Humanoid") == Enum.HumanoidStateType.Freefall then
                        pcall(function()
                            local TargetVelv5 = Prey.Character[getgenv().biosource.Silent.Aimpart]
                            TargetVelv5.Velocity = Vector3.new(TargetVelv5.Velocity.X, (TargetVelv5.Velocity.Y * 5), TargetVelv5.Velocity.Z)
                            TargetVelv5.AssemblyLinearVelocity = Vector3.new(TargetVelv5.Velocity.X, (TargetVelv5.Velocity.Y * 5), TargetVelv5.Velocity.Z)
                        end)
                    end
                    
                    if getgenv().biosource.Resolver.Enabled == true then            
                        pcall(function()
                            local TargetVelv2 = Prey.Character[getgenv().biosource.Silent.Aimpart]
                            TargetVelv2.Velocity = Vector3.new(TargetVelv2.Velocity.X, 0, TargetVelv2.Velocity.Z)
                            TargetVelv2.AssemblyLinearVelocity = Vector3.new(TargetVelv2.Velocity.X, 0, TargetVelv2.Velocity.Z)
                        end)
                    end
                end
            end
        
            if getgenv().biosource.Camlock.Enabled == true then
                if getgenv().biosource.Resolver.Enabled == true and Plr and Plr.Character and Plr.Character:WaitForChild(getgenv().biosource.Camlock.Aimpart) and Plr.Character:WaitForChild("HumanoidRootPart").Velocity.magnitude > 70 then
                    pcall(function()
                        local TargetVelv3 = Plr.Character[getgenv().biosource.Camlock.Aimpart]
                        TargetVelv3.Velocity = Vector3.new(0, 0, 0)
                        TargetVelv3.AssemblyLinearVelocity = Vector3.new(0, 0, 0)
                    end)
                end
                if getgenv().biosource.Checks.AntiGroundShots == true and Plr.Character:FindFirstChild("Humanoid") == Enum.HumanoidStateType.Freefall then
                        pcall(function()
                            local TargetVelv5 = Plr.Character[getgenv().biosource.Camlock.Aimpart]
                            TargetVelv5.Velocity = Vector3.new(TargetVelv5.Velocity.X, (TargetVelv5.Velocity.Y * 5), TargetVelv5.Velocity.Z)
                            TargetVelv5.AssemblyLinearVelocity = Vector3.new(TargetVelv5.Velocity.X, (TargetVelv5.Velocity.Y * 5), TargetVelv5.Velocity.Z)
                        end)
                end
            
                if getgenv().biosource.Resolver.Enabled == true and Plr and Plr.Character and Plr.Character:WaitForChild(getgenv().biosource.Camlock.Aimpart) then
                    pcall(function()
                        local TargetVelv4 = Plr.Character[getgenv().biosource.Camlock.Aimpart]
                        TargetVelv4.Velocity = Vector3.new(TargetVelv4.Velocity.X, 0, TargetVelv4.Velocity.Z)
                        TargetVelv4.AssemblyLinearVelocity = Vector3.new(TargetVelv4.Velocity.X, 0, TargetVelv4.Velocity.Z)
                    end)
                end
            end
        end)
        
        RS.RenderStepped:Connect(function()
            if getgenv().biosource.Silent.Enabled then
                if getgenv().biosource.Checks.Knocked == true and Prey and Prey.Character then 
                    local KOd = Prey.Character:WaitForChild("BodyEffects")["K.O"].Value
                    local Grabbed = Prey.Character:FindFirstChild("GRABBING_CONSTRAINT") ~= nil
        
                    if KOd or Grabbed then
                        Prey = nil
                    end
                end
            end
            if getgenv().biosource.Camlock.Enabled == true then
                if getgenv().biosource.Checks.Knocked == true and Plr and Plr.Character then 
                    local KOd = Plr.Character:WaitForChild("BodyEffects")["K.O"].Value
                    local Grabbed = Plr.Character:FindFirstChild("GRABBING_CONSTRAINT") ~= nil
        
                    if KOd or Grabbed then
                        Plr = nil
                        IsTargetting = false
                    end
                end
                
                if getgenv().biosource.Checks.DisableOnDeath == true and Plr and Plr.Character:FindFirstChild("Humanoid") then
        
                    if Plr.Character.Humanoid.health < 0.1 then
                        Plr = nil
                        IsTargetting = false
                    end
                end
                if getgenv().biosource.Checks.DisableOnDeath == true and Plr and Plr.Character:FindFirstChild("Humanoid") then
        
                    if Client.Character.Humanoid.health < 0.1 then
                        Plr = nil
                        IsTargetting = false
                    end
                end
                if getgenv().biosource.Checks.DisableOutsideFov == true and Plr and Plr.Character and Plr.Character:WaitForChild("HumanoidRootPart") then
        
                    if
                    Camlock.Radius <
                        (Vector2.new(
                            Camera:WorldToScreenPoint(Plr.Character.HumanoidRootPart.Position).X,
                            Camera:WorldToScreenPoint(Plr.Character.HumanoidRootPart.Position).Y
                        ) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude
                     then
                        Plr = nil
                        IsTargetting = false
                    end
                end
                if getgenv().biosource.Camlock.Predict and Plr and Plr.Character and Plr.Character:FindFirstChild(getgenv().biosource.Camlock.Aimpart) then
                    if getgenv().biosource.Camlock.Shake then
                        local Main = CFrame.new(Camera.CFrame.p,Plr.Character[getgenv().biosource.Camlock.Aimpart].Position + Plr.Character[getgenv().biosource.Camlock.Aimpart].Velocity * getgenv().biosource.Camlock.Prediction +
                        Vector3.new(
                            math.random(-getgenv().biosource.Camlock.ShakeValue, getgenv().biosource.Camlock.ShakeValue),
                            math.random(-getgenv().biosource.Camlock.ShakeValue, getgenv().biosource.Camlock.ShakeValue),
                            math.random(-getgenv().biosource.Camlock.ShakeValue, getgenv().biosource.Camlock.ShakeValue)
                        ) * 0.1)
                        Camera.CFrame = Camera.CFrame:Lerp(Main, getgenv().biosource.Camlock.SmoothnessValue / 2, Enum.EasingStyle.Elastic, Enum.EasingDirection.InOut, Enum.EasingStyle.Sine, Enum.EasingDirection.Out)
                    else
                        local Main = CFrame.new(Camera.CFrame.p,Plr.Character[getgenv().biosource.Camlock.Aimpart].Position + Plr.Character[getgenv().biosource.Camlock.Aimpart].Velocity * getgenv().biosource.Camlock.Prediction)
                        Camera.CFrame = Camera.CFrame:Lerp(Main, getgenv().biosource.Camlock.SmoothnessValue / 2, Enum.EasingStyle.Elastic, Enum.EasingDirection.InOut, Enum.EasingStyle.Sine, Enum.EasingDirection.Out)
                    end
                elseif getgenv().biosource.Camlock.Predict == false and Plr and Plr.Character and Plr.Character:FindFirstChild(getgenv().biosource.Camlock.Aimpart) then
                    if getgenv().biosource.Camlock.Shake then
                        local Main = CFrame.new(Camera.CFrame.p,Plr.Character[getgenv().biosource.Camlock.Aimpart].Position +
                        Vector3.new(
                            math.random(-getgenv().biosource.Camlock.ShakeValue, getgenv().biosource.Camlock.ShakeValue),
                            math.random(-getgenv().biosource.Camlock.ShakeValue, getgenv().biosource.Camlock.ShakeValue),
                            math.random(-getgenv().biosource.Camlock.ShakeValue, getgenv().biosource.Camlock.ShakeValue)
                        ) * 0.1)
                        Camera.CFrame = Camera.CFrame:Lerp(Main, getgenv().biosource.Camlock.SmoothnessValue / 2, Enum.EasingStyle.Elastic, Enum.EasingDirection.InOut, Enum.EasingStyle.Sine, Enum.EasingDirection.Out)
                    else
                        local Main = CFrame.new(Camera.CFrame.p,Plr.Character[getgenv().biosource.Camlock.Aimpart].Position)
                        Camera.CFrame = Camera.CFrame:Lerp(Main, getgenv().biosource.Camlock.SmoothnessValue / 2, Enum.EasingStyle.Elastic, Enum.EasingDirection.InOut, Enum.EasingStyle.Sine, Enum.EasingDirection.Out)
                    end
                end
            end
        end)
        
        task.spawn(function ()
            while task.wait() do
                if getgenv().biosource.Silent.Enabled then
                    Prey = ClosestPlrFromMouse()
                end
                if Plr then
                    if getgenv().biosource.Camlock.Enabled and (Plr.Character) and getgenv().biosource.Camlock.NearestCursorAimpart then
                        
                        getgenv().biosource.Camlock.Aimpart = tostring(GetClosestBodyPartV2(Plr.Character))
                    end
                end
                if Prey then
                    if getgenv().biosource.Silent.Enabled and (Prey.Character) and getgenv().biosource.Silent.NearestCursorAimpart then
                        
                        getgenv().biosource.Silent.Aimpart = tostring(GetClosestBodyPart(Prey.Character))
                    end
                end
            end
        end)
        
        getgenv().uhpoop = {
            ["Enabled"] = (getgenv().biosource.GunFov.Enabled),
            ["Double-Barrel SG"] = {["FOV"] = (getgenv().biosource.GunFov.DoubleBarrel)},
            ["DoubleBarrel"] = {["FOV"] = (getgenv().biosource.GunFov.DoubleBarrel)}, 
            ["Revolver"] = {["FOV"] = (getgenv().biosource.GunFov.Revolver)},
            ["SMG"] = {["FOV"] = (getgenv().biosource.GunFov.Smg)}, 
            ["Shotgun"] = {["FOV"] = (getgenv().biosource.GunFov.Shotgun)}, 
            ["TacticalShotgun"] = {["FOV"] = (getgenv().biosource.GunFov.TacticalShotgun)}, 
            ["Silencer"] = {["FOV"] = (getgenv().biosource.GunFov.Silencer)}, 
            
        }                 
        
        local Script = {Functions = {}}
            Script.Functions.getToolName = function(name)
                local split = string.split(string.split(name, "[")[2], "]")[1]
                return split
            end
            Script.Functions.getEquippedWeaponName = function()
                if (Client.Character) and Client.Character:FindFirstChildWhichIsA("Tool") then
                   local Tool =  Client.Character:FindFirstChildWhichIsA("Tool")
                   if string.find(Tool.Name, "%[") and string.find(Tool.Name, "%]") and not string.find(Tool.Name, "Wallet") and not string.find(Tool.Name, "Phone") then
                      return Script.Functions.getToolName(Tool.Name)
                   end
                end
                return nil
            end
            RS.RenderStepped:Connect(function()
            if Script.Functions.getEquippedWeaponName() ~= nil then
                local WeaponSettings = getgenv().uhpoop[Script.Functions.getEquippedWeaponName()]
                if WeaponSettings ~= nil and getgenv().biosource.GunFov.Enabled == true then
                    getgenv().biosource.Fov.Silent.Size = WeaponSettings.FOV
                else
                    getgenv().biosource.Fov.Silent.Size = getgenv().biosource.Fov.Silent.Size
                end
            end
        end)
        
        
        local Player = game:GetService("Players").LocalPlayer
                    local Mouse = Player:GetMouse()
                    local SpeedGlitch = false
                    Mouse.KeyDown:Connect(function(Key)
                        if getgenv().biosource.Macro.Type == "Normal" and getgenv().biosource.Macro.Enabled == true and Key == getgenv().biosource.Macro.Keybind then
                            SpeedGlitch = not SpeedGlitch
                            if SpeedGlitch == true then
                                repeat game:GetService("RunService").Heartbeat:wait()
                                    keypress(0x49)
                                    game:GetService("RunService").Heartbeat:wait()
        
                                    keypress(0x4F)
                                    game:GetService("RunService").Heartbeat:wait()
        
                                    keyrelease(0x49)
                                    game:GetService("RunService").Heartbeat:wait()
        
                                    keyrelease(0x4F)
                                    game:GetService("RunService").Heartbeat:wait()
        
                                until SpeedGlitch == false
                            end
                        end
                    end)
        
        getgenv().predictionTable = {
            {20, 0.12588},
            {30, 0.11911},
            {40, 0.12471},
            {50, 0.12766},
            {60, 0.12731},
            {70, 0.12951},
            {80, 0.13181},
            {90, 0.13573},
            {100, 0.13334},
            {110, 0.14552},
            {120, 0.14376},
            {130, 0.15669},
            {140, 0.12234},
            {150, 0.15214},
        }
        while getgenv().biosource.AutoPrediction.Enabled do
            local ping = game:GetService("Stats").Network.ServerStatsItem["Data Ping"]:GetValueString()
            local pingValue = string.split(ping, " ")[1]
            local pingNumber = tonumber(pingValue-10)
            local prediction = 0
            wait(0.2)
            for i = 1, #predictionTable do
                if pingNumber < predictionTable[i][1] then
                    prediction = predictionTable[i][2]
                    break
                end
        end 
            task.wait(1.89)
            getgenv().biosource.Silent.Prediction = prediction
            task.wait(0.00054)
            ifelse:
            warn('Auto pred has broken.')
        end
print('hi')
