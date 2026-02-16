
-- Carregar Rayfield
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- Tema Exclusivo Dark Nebula
local Theme = {
    TextColor = Color3.fromRGB(255, 255, 255),
    Background = Color3.fromRGB(18, 12, 25),
    Topbar = Color3.fromRGB(98, 0, 234),
    Shadow = Color3.fromRGB(8, 4, 15),
    NotificationBackground = Color3.fromRGB(28, 18, 35),
    NotificationActionsBackground = Color3.fromRGB(157, 78, 221),
    TabBackground = Color3.fromRGB(118, 27, 187),
    TabStroke = Color3.fromRGB(148, 57, 227),
    TabBackgroundSelected = Color3.fromRGB(198, 107, 227),
    TabTextColor = Color3.fromRGB(255, 255, 255),
    SelectedTabTextColor = Color3.fromRGB(0, 0, 0),
    ElementBackground = Color3.fromRGB(38, 24, 48),
    ElementBackgroundHover = Color3.fromRGB(58, 38, 73),
    SecondaryElementBackground = Color3.fromRGB(28, 18, 38),
    ElementStroke = Color3.fromRGB(118, 27, 187),
    SecondaryElementStroke = Color3.fromRGB(88, 8, 138),
    SliderBackground = Color3.fromRGB(148, 57, 227),
    SliderProgress = Color3.fromRGB(198, 107, 227),
    SliderStroke = Color3.fromRGB(157, 78, 221),
    ToggleBackground = Color3.fromRGB(38, 24, 45),
    ToggleEnabled = Color3.fromRGB(148, 57, 227),
    ToggleDisabled = Color3.fromRGB(88, 8, 138),
    ToggleEnabledStroke = Color3.fromRGB(198, 107, 227),
    ToggleDisabledStroke = Color3.fromRGB(118, 27, 187),
    ToggleEnabledOuterStroke = Color3.fromRGB(157, 78, 221),
    ToggleDisabledOuterStroke = Color3.fromRGB(68, 38, 83),
    DropdownSelected = Color3.fromRGB(118, 27, 187),
    DropdownUnselected = Color3.fromRGB(38, 24, 48),
    InputBackground = Color3.fromRGB(38, 24, 45),
    InputStroke = Color3.fromRGB(118, 27, 187),
    PlaceholderColor = Color3.fromRGB(198, 107, 227)
}

-- Criar janela principal
local Window = Rayfield:CreateWindow({
    Name = "☯ OMEGA VISION - ELITE FRAMEWORK ☯",
    Icon = 0,
    LoadingTitle = "OMEGA VISION",
    LoadingSubtitle = "Inicializando módulos quânticos...",
    ShowText = "Omega Hub",
    Theme = Theme,
    ToggleUIKeybind = "K",
    DisableRayfieldPrompts = false,
    DisableBuildWarnings = true,
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "OmegaVision",
        FileName = "QuantumConfig"
    },
    Discord = {
        Enabled = false,
        Invite = "omega",
        RememberJoins = true
    },
    KeySystem = false
})

-- Sistema de Notificações Premium
local function OmegaNotify(title, content, duration, icon)
    Rayfield:Notify({
        Title = "⚡ " .. title,
        Content = content,
        Duration = duration or 3,
        Image = icon or "info"
    })
end

OmegaNotify("OMEGA VISION", "Framework inicializado com sucesso!\nBem-vindo ao futuro.", 5, "zap")

-- Variáveis Globais
local player = game:GetService("Players").LocalPlayer
local mouse = player:GetMouse()
local uis = game:GetService("UserInputService")
local runService = game:GetService("RunService")
local workspace = game:GetService("Workspace")
local camera = workspace.CurrentCamera

-- Módulos de Controle
local Modules = {
    Fly = {Enabled = false, Speed = 50},
    Speed = {Enabled = false, Value = 50},
    Jump = {Enabled = false, Power = 50},
    InfiniteJump = false,
    Gravity = {Enabled = false, Value = 196.2},
    Noclip = false,
    ESP = {Enabled = false, Wallhack = false},
    Aim = {Enabled = false, Circle = false, Assist = false, Triggerbot = false, FOV = 150, Smoothness = 5, Mode = "Cabeça"},
    Teleport = {Target = nil},
    Autofarm = {Enabled = false, Distance = 30}
}

-- Criar abas
local Tabs = {
    Movement = Window:CreateTab("Movimentação", "activity"),
    Visual = Window:CreateTab("Visualização", "eye"),
    Combat = Window:CreateTab("Combate", "target"),
    Teleport = Window:CreateTab("Teleporte", "map-pin"),
    Automation = Window:CreateTab("Automação", "bot"),
    Information = Window:CreateTab("Informações", "info")
}

--[[ ========== MÓDULO DE MOVIMENTAÇÃO ========== ]]
local MoveSection = Tabs.Movement:CreateSection("SISTEMA DE LOCOMOÇÃO")

-- Fly System
local FlyToggle = Tabs.Movement:CreateToggle({
    Name = "⚡ MODO VOO",
    CurrentValue = false,
    Flag = "Omega_Fly",
    Callback = function(value)
        Modules.Fly.Enabled = value
        local character = player.Character
        
        if not character or not character:FindFirstChild("HumanoidRootPart") then
            OmegaNotify("ERRO", "Personagem não encontrado!", 3, "alert-triangle")
            return
        end
        
        local hrp = character.HumanoidRootPart
        local humanoid = character:FindFirstChild("Humanoid")
        
        if value then
            if humanoid then humanoid.PlatformStand = true end
            
            local bv = Instance.new("BodyVelocity")
            bv.MaxForce = Vector3.new(4000, 4000, 4000)
            bv.Parent = hrp
            
            local bg = Instance.new("BodyGyro")
            bg.MaxTorque = Vector3.new(4000, 4000, 4000)
            bg.P = 1000
            bg.Parent = hrp
            
            runService:BindToRenderStep("FlyLoop", Enum.RenderPriority.Character.Value, function()
                if not Modules.Fly.Enabled or not hrp or not hrp.Parent then return end
                
                local move = Vector3.new()
                local cam = camera
                
                if uis:IsKeyDown(Enum.KeyCode.W) then move = move + cam.CFrame.LookVector end
                if uis:IsKeyDown(Enum.KeyCode.S) then move = move - cam.CFrame.LookVector end
                if uis:IsKeyDown(Enum.KeyCode.A) then move = move - cam.CFrame.RightVector end
                if uis:IsKeyDown(Enum.KeyCode.D) then move = move + cam.CFrame.RightVector end
                if uis:IsKeyDown(Enum.KeyCode.Space) then move = move + Vector3.new(0, 1, 0) end
                if uis:IsKeyDown(Enum.KeyCode.LeftShift) then move = move - Vector3.new(0, 1, 0) end
                
                bv.Velocity = move * Modules.Fly.Speed
                bg.CFrame = CFrame.new(hrp.Position, hrp.Position + cam.CFrame.LookVector)
            end)
            
            OmegaNotify("VOO ATIVADO", "Velocidade: " .. Modules.Fly.Speed .. " studs/s", 3, "plane")
        else
            if hrp:FindFirstChild("BodyVelocity") then hrp.BodyVelocity:Destroy() end
            if hrp:FindFirstChild("BodyGyro") then hrp.BodyGyro:Destroy() end
            if humanoid then humanoid.PlatformStand = false end
            runService:UnbindFromRenderStep("FlyLoop")
            OmegaNotify("VOO DESATIVADO", "Modo normal restaurado", 3, "plane-off")
        end
    end
})

local FlySpeed = Tabs.Movement:CreateSlider({
    Name = "VELOCIDADE DE VOO",
    Range = {10, 500},
    Increment = 10,
    Suffix = "studs/s",
    CurrentValue = 50,
    Flag = "Omega_FlySpeed",
    Callback = function(value) Modules.Fly.Speed = value end
})

-- Speed System
local SpeedToggle = Tabs.Movement:CreateToggle({
    Name = "⚡ SUPER VELOCIDADE",
    CurrentValue = false,
    Flag = "Omega_Speed",
    Callback = function(value)
        Modules.Speed.Enabled = value
        local character = player.Character
        
        if not character or not character:FindFirstChild("Humanoid") then
            OmegaNotify("ERRO", "Personagem não encontrado!", 3, "alert-triangle")
            return
        end
        
        local humanoid = character.Humanoid
        
        if value then
            humanoid:SetAttribute("OriginalSpeed", humanoid.WalkSpeed)
            humanoid.WalkSpeed = Modules.Speed.Value
            OmegaNotify("VELOCIDADE", "Ativada: " .. Modules.Speed.Value .. " studs/s", 3, "zap")
        else
            humanoid.WalkSpeed = humanoid:GetAttribute("OriginalSpeed") or 16
            OmegaNotify("VELOCIDADE", "Velocidade normal restaurada", 3, "zap-off")
        end
    end
})

local SpeedValue = Tabs.Movement:CreateSlider({
    Name = "VELOCIDADE",
    Range = {16, 500},
    Increment = 10,
    Suffix = "studs/s",
    CurrentValue = 50,
    Flag = "Omega_SpeedValue",
    Callback = function(value)
        Modules.Speed.Value = value
        if Modules.Speed.Enabled and player.Character and player.Character:FindFirstChild("Humanoid") then
            player.Character.Humanoid.WalkSpeed = value
        end
    end
})

-- Jump System
local JumpToggle = Tabs.Movement:CreateToggle({
    Name = "⚡ SUPER PULO",
    CurrentValue = false,
    Flag = "Omega_Jump",
    Callback = function(value)
        Modules.Jump.Enabled = value
        local character = player.Character
        
        if not character or not character:FindFirstChild("Humanoid") then
            OmegaNotify("ERRO", "Personagem não encontrado!", 3, "alert-triangle")
            return
        end
        
        local humanoid = character.Humanoid
        
        if value then
            humanoid:SetAttribute("OriginalJump", humanoid.JumpPower)
            humanoid.JumpPower = Modules.Jump.Power
            OmegaNotify("PULO", "Altura: " .. Modules.Jump.Power, 3, "arrow-up")
        else
            humanoid.JumpPower = humanoid:GetAttribute("OriginalJump") or 50
            OmegaNotify("PULO", "Altura normal restaurada", 3, "arrow-down")
        end
    end
})

local JumpValue = Tabs.Movement:CreateSlider({
    Name = "ALTURA DO PULO",
    Range = {50, 500},
    Increment = 10,
    Suffix = "força",
    CurrentValue = 50,
    Flag = "Omega_JumpValue",
    Callback = function(value)
        Modules.Jump.Power = value
        if Modules.Jump.Enabled and player.Character and player.Character:FindFirstChild("Humanoid") then
            player.Character.Humanoid.JumpPower = value
        end
    end
})

-- Infinite Jump
local InfiniteJump = Tabs.Movement:CreateToggle({
    Name = "♾️ PULO INFINITO",
    CurrentValue = false,
    Flag = "Omega_InfiniteJump",
    Callback = function(value)
        Modules.InfiniteJump = value
        if value then
            local connection
            connection = uis.JumpRequest:Connect(function()
                if Modules.InfiniteJump and player.Character and player.Character:FindFirstChild("Humanoid") then
                    player.Character.Humanoid:ChangeState("Jumping")
                end
            end)
            _G.InfiniteJumpConn = connection
            OmegaNotify("PULO INFINITO", "Ativado", 3, "repeat")
        else
            if _G.InfiniteJumpConn then
                _G.InfiniteJumpConn:Disconnect()
                _G.InfiniteJumpConn = nil
            end
            OmegaNotify("PULO INFINITO", "Desativado", 3, "x")
        end
    end
})

-- Gravity
local GravityToggle = Tabs.Movement:CreateToggle({
    Name = "🌍 CONTROLE DE GRAVIDADE",
    CurrentValue = false,
    Flag = "Omega_Gravity",
    Callback = function(value)
        Modules.Gravity.Enabled = value
        if value then
            workspace.Gravity = Modules.Gravity.Value
            OmegaNotify("GRAVIDADE", "Valor: " .. Modules.Gravity.Value, 3, "moon")
        else
            workspace.Gravity = 196.2
            OmegaNotify("GRAVIDADE", "Gravidade normal restaurada", 3, "sun")
        end
    end
})

local GravityValue = Tabs.Movement:CreateSlider({
    Name = "FORÇA GRAVITACIONAL",
    Range = {0, 500},
    Increment = 10,
    Suffix = "força",
    CurrentValue = 196.2,
    Flag = "Omega_GravityValue",
    Callback = function(value)
        Modules.Gravity.Value = value
        if Modules.Gravity.Enabled then
            workspace.Gravity = value
        end
    end
})

-- Noclip
local NoclipToggle = Tabs.Movement:CreateToggle({
    Name = "🌀 MODO NOCLIP",
    CurrentValue = false,
    Flag = "Omega_Noclip",
    Callback = function(value)
        Modules.Noclip = value
        if value then
            runService:BindToRenderStep("NoclipLoop", Enum.RenderPriority.Character.Value, function()
                if Modules.Noclip and player.Character then
                    for _, part in ipairs(player.Character:GetDescendants()) do
                        if part:IsA("BasePart") then
                            part.CanCollide = false
                        end
                    end
                end
            end)
            OmegaNotify("NOCLIP", "Atravesse paredes!", 3, "wand")
        else
            runService:UnbindFromRenderStep("NoclipLoop")
            if player.Character then
                for _, part in ipairs(player.Character:GetDescendants()) do
                    if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
                        part.CanCollide = true
                    end
                end
            end
            OmegaNotify("NOCLIP", "Modo normal", 3, "x")
        end
    end
})

--[[ ========== MÓDULO DE VISUALIZAÇÃO ========== ]]
local VisualSection = Tabs.Visual:CreateSection("SISTEMA TÁTICO")

-- ESP System
local ESPObjects = {}

local function CreateESP(player)
    if ESPObjects[player] then return end
    
    local esp = {
        Box = Drawing.new("Square"),
        Name = Drawing.new("Text"),
        Distance = Drawing.new("Text"),
        Health = Drawing.new("Text")
    }
    
    esp.Box.Visible = false
    esp.Box.Color = Color3.fromRGB(148, 57, 227)
    esp.Box.Thickness = 2
    esp.Box.Filled = false
    
    esp.Name.Visible = false
    esp.Name.Color = Color3.fromRGB(255, 255, 255)
    esp.Name.Size = 16
    esp.Name.Center = true
    esp.Name.Outline = true
    esp.Name.Font = 2
    
    esp.Distance.Visible = false
    esp.Distance.Color = Color3.fromRGB(198, 107, 227)
    esp.Distance.Size = 14
    esp.Distance.Center = true
    esp.Distance.Outline = true
    esp.Distance.Font = 2
    
    esp.Health.Visible = false
    esp.Health.Color = Color3.fromRGB(0, 255, 0)
    esp.Health.Size = 14
    esp.Health.Center = true
    esp.Health.Outline = true
    esp.Health.Font = 2
    
    ESPObjects[player] = esp
end

local ESPToggle = Tabs.Visual:CreateToggle({
    Name = "👁️ SISTEMA ESP",
    CurrentValue = false,
    Flag = "Omega_ESP",
    Callback = function(value)
        Modules.ESP.Enabled = value
        
        if value then
            for _, plr in ipairs(game:GetService("Players"):GetPlayers()) do
                if plr ~= player then CreateESP(plr) end
            end
            
            runService:BindToRenderStep("ESPLoop", Enum.RenderPriority.Camera.Value, function()
                if not Modules.ESP.Enabled then return end
                
                for plr, esp in pairs(ESPObjects) do
                    if plr and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") and plr.Character:FindFirstChild("Humanoid") then
                        local hrp = plr.Character.HumanoidRootPart
                        local hum = plr.Character.Humanoid
                        local pos, onScreen = camera:WorldToViewportPoint(hrp.Position)
                        
                        if onScreen then
                            local dist = (camera.CFrame.Position - hrp.Position).Magnitude
                            local scale = 500 / dist
                            local size = Vector2.new(scale * 2, scale * 3)
                            
                            esp.Box.Position = Vector2.new(pos.X - size.X/2, pos.Y - size.Y/2)
                            esp.Box.Size = size
                            esp.Box.Visible = true
                            
                            esp.Name.Position = Vector2.new(pos.X, pos.Y - size.Y/2 - 20)
                            esp.Name.Text = plr.Name
                            esp.Name.Visible = true
                            
                            esp.Distance.Position = Vector2.new(pos.X, pos.Y - size.Y/2 - 5)
                            esp.Distance.Text = math.floor(dist) .. " studs"
                            esp.Distance.Visible = true
                            
                            local healthColor = Color3.fromRGB(255 * (1 - hum.Health/hum.MaxHealth), 255 * (hum.Health/hum.MaxHealth), 0)
                            esp.Health.Color = healthColor
                            esp.Health.Position = Vector2.new(pos.X, pos.Y + size.Y/2 + 5)
                            esp.Health.Text = math.floor(hum.Health) .. "/" .. math.floor(hum.MaxHealth) .. " HP"
                            esp.Health.Visible = true
                        else
                            esp.Box.Visible = false
                            esp.Name.Visible = false
                            esp.Distance.Visible = false
                            esp.Health.Visible = false
                        end
                    else
                        esp.Box.Visible = false
                        esp.Name.Visible = false
                        esp.Distance.Visible = false
                        esp.Health.Visible = false
                    end
                end
            end)
            
            OmegaNotify("ESP ATIVADO", "Visualização tática", 3, "eye")
        else
            runService:UnbindFromRenderStep("ESPLoop")
            for _, esp in pairs(ESPObjects) do
                esp.Box.Visible = false
                esp.Box:Remove()
                esp.Name.Visible = false
                esp.Name:Remove()
                esp.Distance.Visible = false
                esp.Distance:Remove()
                esp.Health.Visible = false
                esp.Health:Remove()
            end
            ESPObjects = {}
            OmegaNotify("ESP DESATIVADO", "Visualização normal", 3, "eye-off")
        end
    end
})

-- Wallhack
local WallhackToggle = Tabs.Visual:CreateToggle({
    Name = "🧿 WALLHACK",
    CurrentValue = false,
    Flag = "Omega_Wallhack",
    Callback = function(value)
        Modules.ESP.Wallhack = value
        
        if value then
            for _, plr in ipairs(game:GetService("Players"):GetPlayers()) do
                if plr ~= player and plr.Character then
                    local highlight = Instance.new("Highlight")
                    highlight.Name = "OmegaWallhack"
                    highlight.FillColor = Color3.fromRGB(148, 57, 227)
                    highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                    highlight.FillTransparency = 0.5
                    highlight.Parent = plr.Character
                end
            end
            
            game:GetService("Players").PlayerAdded:Connect(function(newPlayer)
                newPlayer.CharacterAdded:Connect(function(char)
                    if Modules.ESP.Wallhack then
                        wait(1)
                        local highlight = Instance.new("Highlight")
                        highlight.Name = "OmegaWallhack"
                        highlight.FillColor = Color3.fromRGB(148, 57, 227)
                        highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                        highlight.FillTransparency = 0.5
                        highlight.Parent = char
                    end
                end)
            end)
            
            OmegaNotify("WALLHACK", "Visão através de paredes", 3, "eye")
        else
            for _, plr in ipairs(game:GetService("Players"):GetPlayers()) do
                if plr.Character and plr.Character:FindFirstChild("OmegaWallhack") then
                    plr.Character.OmegaWallhack:Destroy()
                end
            end
            OmegaNotify("WALLHACK", "Visão normal", 3, "eye-off")
        end
    end
})

--[[ ========== MÓDULO DE COMBATE ========== ]]
local CombatSection = Tabs.Combat:CreateSection("SISTEMA TÁTICO DE COMBATE")

-- Aim Variables
local aimCircle = Drawing.new("Circle")
aimCircle.Visible = false
aimCircle.Radius = Modules.Aim.FOV
aimCircle.Color = Color3.fromRGB(148, 57, 227)
aimCircle.Thickness = 2
aimCircle.NumSides = 64
aimCircle.Filled = false
aimCircle.Transparency = 1
aimCircle.Position = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2)

camera:GetPropertyChangedSignal("ViewportSize"):Connect(function()
    aimCircle.Position = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2)
end)

local function GetTargetPart(character)
    if not character then return nil end
    if Modules.Aim.Mode == "Cabeça" then
        return character:FindFirstChild("Head") or character:FindFirstChild("HumanoidRootPart")
    elseif Modules.Aim.Mode == "Tronco" then
        return character:FindFirstChild("Torso") or character:FindFirstChild("UpperTorso") or character:FindFirstChild("HumanoidRootPart")
    else
        local parts = {"Head", "Torso", "UpperTorso", "LowerTorso", "HumanoidRootPart"}
        for _, part in ipairs(parts) do
            local found = character:FindFirstChild(part)
            if found then return found end
        end
    end
    return character:FindFirstChild("HumanoidRootPart")
end

local function GetClosestPlayer()
    local closest = nil
    local closestDist = Modules.Aim.FOV
    local center = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2)
    
    for _, plr in ipairs(game:GetService("Players"):GetPlayers()) do
        if plr ~= player and plr.Character then
            local part = GetTargetPart(plr.Character)
            if part then
                local pos, onScreen = camera:WorldToViewportPoint(part.Position)
                if onScreen then
                    local dist = (Vector2.new(pos.X, pos.Y) - center).Magnitude
                    if dist < closestDist then
                        closestDist = dist
                        closest = plr
                    end
                end
            end
        end
    end
    return closest
end

-- Aim Toggles
local AimToggle = Tabs.Combat:CreateToggle({
    Name = "🎯 SISTEMA DE MIRA",
    CurrentValue = false,
    Flag = "Omega_Aim",
    Callback = function(value)
        Modules.Aim.Enabled = value
        OmegaNotify(value and "MIRA ATIVADA" or "MIRA DESATIVADA", value and "Pronta para ação" or "Desligada", 3, "target")
    end
})

local CircleToggle = Tabs.Combat:CreateToggle({
    Name = "⭕ CÍRCULO TÁTICO (FOV)",
    CurrentValue = false,
    Flag = "Omega_Circle",
    Callback = function(value)
        Modules.Aim.Circle = value
        aimCircle.Visible = value
    end
})

local AimbotToggle = Tabs.Combat:CreateToggle({
    Name = "🎯 AIMBOT AUTOMÁTICO",
    CurrentValue = false,
    Flag = "Omega_Aimbot",
    Callback = function(value)
        Modules.Aim.Assist = value and Modules.Aim.Enabled
        if value and not Modules.Aim.Enabled then
            OmegaNotify("AVISO", "Ative o sistema de mira primeiro!", 3, "alert-triangle")
            AimbotToggle:Set(false)
        end
    end
})

local TriggerbotToggle = Tabs.Combat:CreateToggle({
    Name = "🔫 TRIGGERBOT",
    CurrentValue = false,
    Flag = "Omega_Triggerbot",
    Callback = function(value)
        Modules.Aim.Triggerbot = value
        OmegaNotify(value and "TRIGGERBOT ATIVADO" or "TRIGGERBOT DESATIVADO", value and "Disparo automático" or "Disparo manual", 3, "mouse-pointer")
    end
})

-- Aim Options
local AimMode = Tabs.Combat:CreateDropdown({
    Name = "🎯 MODO DE MIRA",
    Options = {"Cabeça", "Tronco", "Aleatório"},
    CurrentOption = {"Cabeça"},
    MultipleOptions = false,
    Flag = "Omega_AimMode",
    Callback = function(options) Modules.Aim.Mode = options[1] end
})

local FOVSlider = Tabs.Combat:CreateSlider({
    Name = "⭕ CAMPO DE VISÃO (FOV)",
    Range = {50, 500},
    Increment = 10,
    Suffix = "px",
    CurrentValue = 150,
    Flag = "Omega_FOV",
    Callback = function(value)
        Modules.Aim.FOV = value
        aimCircle.Radius = value
    end
})

local SmoothnessSlider = Tabs.Combat:CreateSlider({
    Name = "✨ SUAVIDADE DA MIRA",
    Range = {1, 20},
    Increment = 1,
    Suffix = "%",
    CurrentValue = 5,
    Flag = "Omega_Smoothness",
    Callback = function(value) Modules.Aim.Smoothness = value end
})

-- Aim Loop
runService:BindToRenderStep("AimLoop", Enum.RenderPriority.Input.Value, function()
    if not Modules.Aim.Enabled then
        if Modules.Aim.Circle then aimCircle.Visible = true end
        return
    end
    
    aimCircle.Visible = Modules.Aim.Circle
    
    if Modules.Aim.Assist then
        local target = GetClosestPlayer()
        if target and target.Character then
            local part = GetTargetPart(target.Character)
            if part then
                local lookVector = (part.Position - camera.CFrame.Position).Unit
                local newCF = CFrame.lookAt(camera.CFrame.Position, camera.CFrame.Position + lookVector)
                camera.CFrame = camera.CFrame:Lerp(newCF, Modules.Aim.Smoothness / 10)
            end
        end
    end
    
    if Modules.Aim.Triggerbot then
        local center = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2)
        for _, plr in ipairs(game:GetService("Players"):GetPlayers()) do
            if plr ~= player and plr.Character then
                local part = GetTargetPart(plr.Character)
                if part then
                    local pos, onScreen = camera:WorldToViewportPoint(part.Position)
                    if onScreen then
                        local dist = (Vector2.new(pos.X, pos.Y) - center).Magnitude
                        if dist < Modules.Aim.FOV then
                            mouse1press()
                            wait(0.1)
                            mouse1release()
                            break
                        end
                    end
                end
            end
        end
    end
end)

--[[ ========== MÓDULO DE TELEPORTE ========== ]]
local TeleportSection = Tabs.Teleport:CreateSection("DESLOCAMENTO ESPACIAL")

local PlayerList = {}
local SelectedPlayer = nil

local function UpdatePlayerList()
    PlayerList = {}
    for _, p in ipairs(game:GetService("Players"):GetPlayers()) do
        if p ~= player then
            table.insert(PlayerList, p.Name)
        end
    end
    if #PlayerList == 0 then
        table.insert(PlayerList, "Nenhum jogador")
    end
    return PlayerList
end

local PlayerDropdown = Tabs.Teleport:CreateDropdown({
    Name = "🎯 SELECIONAR JOGADOR",
    Options = UpdatePlayerList(),
    CurrentOption = {UpdatePlayerList()[1]},
    MultipleOptions = false,
    Flag = "Omega_PlayerSelect",
    Callback = function(options) SelectedPlayer = options[1] end
})

local TpToPlayer = Tabs.Teleport:CreateButton({
    Name = "📡 TELEPORTAR PARA JOGADOR",
    Callback = function()
        if not SelectedPlayer or SelectedPlayer == "Nenhum jogador" then
            OmegaNotify("ERRO", "Selecione um jogador válido!", 3, "alert-circle")
            return
        end
        
        local target = game:GetService("Players"):FindFirstChild(SelectedPlayer)
        if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
            if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                player.Character.HumanoidRootPart.CFrame = target.Character.HumanoidRootPart.CFrame
                OmegaNotify("TELEPORTADO", "Para " .. SelectedPlayer, 3, "map-pin")
            end
        end
    end
})

local TpToMouse = Tabs.Teleport:CreateButton({
    Name = "🖱️ TELEPORTAR PARA MOUSE",
    Callback = function()
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local target = mouse.Hit
            if target then
                player.Character.HumanoidRootPart.CFrame = target
                OmegaNotify("TELEPORTADO", "Para posição do mouse", 3, "mouse-pointer")
            end
        end
    end
})

local RefreshPlayers = Tabs.Teleport:CreateButton({
    Name = "🔄 ATUALIZAR LISTA",
    Callback = function()
        PlayerDropdown:Refresh(UpdatePlayerList())
        OmegaNotify("LISTA ATUALIZADA", #PlayerList - 1 .. " jogadores encontrados", 2, "refresh-cw")
    end
})

--[[ ========== MÓDULO DE AUTOMAÇÃO ========== ]]
local AutoSection = Tabs.Automation:CreateSection("AUTOMAÇÃO INTELIGENTE")

local AutoFarmToggle = Tabs.Automation:CreateToggle({
    Name = "🤖 AUTOFARM",
    CurrentValue = false,
    Flag = "Omega_Autofarm",
    Callback = function(value)
        Modules.Autofarm.Enabled = value
        
        if value then
            runService:BindToRenderStep("AutoFarmLoop", Enum.RenderPriority.Character.Value, function()
                if not Modules.Autofarm.Enabled then return end
                if not player.Character or not player.Character:FindFirstChild("HumanoidRootPart") then return end
                
                local hrp = player.Character.HumanoidRootPart
                
                -- Procurar por objetos coletáveis
                for _, obj in ipairs(workspace:GetDescendants()) do
                    if obj:IsA("BasePart") and obj:FindFirstChild("TouchInterest") then
                        local dist = (obj.Position - hrp.Position).Magnitude
                        if dist < Modules.Autofarm.Distance then
                            hrp.CFrame = CFrame.new(obj.Position)
                        end
                    end
                end
                
                -- Procurar por inimigos próximos
                for _, plr in ipairs(game:GetService("Players"):GetPlayers()) do
                    if plr ~= player and plr.Character and plr.Character:FindFirstChild("Humanoid") and plr.Character.Humanoid.Health > 0 then
                        local dist = (plr.Character.HumanoidRootPart.Position - hrp.Position).Magnitude
                        if dist < Modules.Autofarm.Distance then
                            -- Simular ataque (genérico)
                            local args = {plr.Character.HumanoidRootPart, plr.Character.Humanoid}
                            local remote = game:GetService("ReplicatedStorage"):FindFirstChildWhichIsA("RemoteEvent")
                            if remote then
                                remote:FireServer(unpack(args))
                            end
                        end
                    end
                end
            end)
            
            OmegaNotify("AUTOFARM", "Ativado", 3, "bot")
        else
            runService:UnbindFromRenderStep("AutoFarmLoop")
            OmegaNotify("AUTOFARM", "Desativado", 3, "x")
        end
    end
})

local AutoDistance = Tabs.Automation:CreateSlider({
    Name = "DISTÂNCIA DE FARM",
    Range = {10, 100},
    Increment = 5,
    Suffix = "studs",
    CurrentValue = 30,
    Flag = "Omega_AutoDistance",
    Callback = function(value) Modules.Autofarm.Distance = value end
})

--[[ ========== MÓDULO DE INFORMAÇÕES ========== ]]
local InfoSection = Tabs.Information:CreateSection("MONITORAMENTO EM TEMPO REAL")

local GameName = Tabs.Information:CreateLabel(
    "🎮 Jogo: " .. game:GetService("MarketplaceService"):GetProductInfo(game.PlaceId).Name,
    "gamepad-2",
    Color3.fromRGB(255, 255, 255),
    false
)

local PlaceID = Tabs.Information:CreateLabel(
    "📋 Place ID: " .. game.PlaceId,
    "hash",
    Color3.fromRGB(255, 255, 255),
    false
)

local PlayerCount = Tabs.Information:CreateLabel(
    "👥 Jogadores: " .. #game:GetService("Players"):GetPlayers() .. "/" .. game:GetService("Players").MaxPlayers,
    "users",
    Color3.fromRGB(255, 255, 255),
    false
)

local ServerTime = Tabs.Information:CreateLabel(
    "⏱️ Tempo de Servidor: Carregando...",
    "clock",
    Color3.fromRGB(255, 255, 255),
    false
)

local FPSLabel = Tabs.Information:CreateLabel(
    "📊 FPS: Calculando...",
    "activity",
    Color3.fromRGB(255, 255, 255),
    false
)

-- FPS Counter
local frameCount = 0
local lastTime = tick()
local fps = 0

runService.RenderStepped:Connect(function()
    frameCount = frameCount + 1
    local currentTime = tick()
    if currentTime - lastTime >= 1 then
        fps = frameCount
        frameCount = 0
        lastTime = currentTime
        FPSLabel:Set("📊 FPS: " .. fps, "activity", Color3.fromRGB(255, 255, 255), false)
    end
end)

-- Atualizações periódicas
spawn(function()
    while true do
        wait(1)
        PlayerCount:Set(
            "👥 Jogadores: " .. #game:GetService("Players"):GetPlayers() .. "/" .. game:GetService("Players").MaxPlayers,
            "users",
            Color3.fromRGB(255, 255, 255),
            false
        )
        
        local time = math.floor(workspace.DistributedGameTime)
        local minutes = math.floor(time / 60)
        local seconds = time % 60
        ServerTime:Set(
            string.format("⏱️ Tempo de Servidor: %02d:%02d", minutes, seconds),
            "clock",
            Color3.fromRGB(255, 255, 255),
            false
        )
    end
end)

-- Informações adicionais
Tabs.Information:CreateParagraph({
    Title = "ℹ️ SOBRE O FRAMEWORK",
    Content = "Omega Vision v2.5.0\nExecutor: " .. identifyexecutor() .. "\nTema: Dark Nebula\nTecnologia: Rayfield v2.0\nDesenvolvedor: NebulaTech™"
})

--[[ ========== CONFIGURAÇÕES ========== ]]
local ConfigTab = Window:CreateTab("Configurações", "settings")
local ConfigSection = ConfigTab:CreateSection("AJUSTES DO SISTEMA")

ConfigTab:CreateButton({
    Name = "🔄 REINICIAR INTERFACE",
    Callback = function()
        OmegaNotify("REINICIANDO", "Em 3 segundos...", 3, "refresh-cw")
        wait(3)
        loadstring(game:HttpGet('https://sirius.menu/rayfield'))()
    end
})

ConfigTab:CreateButton({
    Name = "❌ DESTRUIR INTERFACE",
    Callback = function()
        Rayfield:Destroy()
    end
})

-- Notificação final
OmegaNotify("OMEGA VISION", "Todos os módulos carregados!\nPressione K para abrir/fechar.", 5, "zap")

-- Atualização inicial da lista de jogadores
UpdatePlayerList()
