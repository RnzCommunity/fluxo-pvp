-- ============================================
-- FLUXO PVP - FULL PACK
-- Aimlock 100% + Triggerbot + ESP + Auto Reload
-- Support Delta Executor (Android)
-- ============================================

local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "Fluxo PVP | Full Pack",
   LoadingTitle = "Memuat script...",
   LoadingSubtitle = "by Kamu",
   ConfigurationSaving = { Enabled = false }
})

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- VARIABEL GLOBAL
_G.AimbotEnabled = false
_G.TeamCheck = true
_G.WallCheck = false
_G.LockPart = "Head"
_G.FOVRadius = 300

_G.TriggerbotEnabled = false
_G.TriggerDelay = 0.05

_G.ESPEnabled = false
_G.ESPColor = Color3.fromRGB(255, 50, 50)
_G.ESPTeamColor = false
_G.ESPName = true
_G.ESPDistance = true

_G.AutoReloadEnabled = false

-- ============================================
-- TAB 1: AIMLOCK
-- ============================================
local AimTab = Window:CreateTab("Aimlock", 4483362458)

AimTab:CreateToggle({
   Name = "Aimlock 100% (FULL LOCK)",
   CurrentValue = false,
   Flag = "AimbotToggle",
   Callback = function(Value) _G.AimbotEnabled = Value end,
})

AimTab:CreateToggle({
   Name = "Team Check",
   CurrentValue = true,
   Flag = "TeamCheck",
   Callback = function(Value) _G.TeamCheck = Value end,
})

AimTab:CreateToggle({
   Name = "Wall Check (Anti-Tembus)",
   CurrentValue = false,
   Flag = "WallCheck",
   Callback = function(Value) _G.WallCheck = Value end,
})

AimTab:CreateDropdown({
   Name = "Lock Part",
   Options = {"Head", "Torso", "HumanoidRootPart"},
   CurrentOption = "Head",
   Flag = "LockPart",
   Callback = function(Option) _G.LockPart = Option end,
})

AimTab:CreateSlider({
   Name = "FOV Radius",
   Range = {50, 800},
   Increment = 10,
   Suffix = "px",
   CurrentValue = 300,
   Flag = "FOVSlider",
   Callback = function(Value) _G.FOVRadius = Value end,
})

-- ============================================
-- TAB 2: TRIGGERBOT
-- ============================================
local TriggerTab = Window:CreateTab("Triggerbot", 4483362458)

TriggerTab:CreateToggle({
   Name = "Triggerbot (Auto-Shoot)",
   CurrentValue = false,
   Flag = "TriggerToggle",
   Callback = function(Value) _G.TriggerbotEnabled = Value end,
})

TriggerTab:CreateSlider({
   Name = "Delay (detik)",
   Range = {0.01, 0.5},
   Increment = 0.01,
   Suffix = "s",
   CurrentValue = 0.05,
   Flag = "TriggerDelay",
   Callback = function(Value) _G.TriggerDelay = Value end,
})

-- ============================================
-- TAB 3: ESP
-- ============================================
local ESPTab = Window:CreateTab("ESP", 4483362458)

ESPTab:CreateToggle({
   Name = "ESP Box",
   CurrentValue = false,
   Flag = "ESPToggle",
   Callback = function(Value) _G.ESPEnabled = Value end,
})

ESPTab:CreateToggle({
   Name = "Warna Sesuai Tim",
   CurrentValue = false,
   Flag = "ESPTeamColor",
   Callback = function(Value) _G.ESPTeamColor = Value end,
})

ESPTab:CreateColorPicker({
   Name = "Warna ESP",
   Color = Color3.fromRGB(255, 50, 50),
   Flag = "ESPColor",
   Callback = function(Value) _G.ESPColor = Value end,
})

ESPTab:CreateToggle({
   Name = "Tampilkan Nama",
   CurrentValue = true,
   Flag = "ESPName",
   Callback = function(Value) _G.ESPName = Value end,
})

ESPTab:CreateToggle({
   Name = "Tampilkan Jarak",
   CurrentValue = true,
   Flag = "ESPDistance",
   Callback = function(Value) _G.ESPDistance = Value end,
})

-- ============================================
-- TAB 4: MISC
-- ============================================
local MiscTab = Window:CreateTab("Misc", 4483362458)

MiscTab:CreateToggle({
   Name = "Auto Reload",
   CurrentValue = false,
   Flag = "AutoReload",
   Callback = function(Value) _G.AutoReloadEnabled = Value end,
})

MiscTab:CreateButton({
   Name = "Respawn Character",
   Callback = function()
       LocalPlayer.Character:BreakJoints()
   end,
})

-- ============================================
-- FOV CIRCLE VISUAL
-- ============================================
local Circle = Drawing.new("Circle")
Circle.Color = Color3.fromRGB(255, 50, 50)
Circle.Thickness = 1.5
Circle.Transparency = 0.8
Circle.Filled = false
Circle.Visible = false

-- ============================================
-- FUNGSI HELPER
-- ============================================
local function IsVisible(targetPart)
    if not _G.WallCheck then return true end
    local rayOrigin = Camera.CFrame.Position
    local direction = (targetPart.Position - rayOrigin)
    local rayParams = RaycastParams.new()
    rayParams.FilterType = Enum.RaycastFilterType.Exclude
    rayParams.FilterDescendantsInstances = {LocalPlayer.Character, Camera}
    local result = workspace:Raycast(rayOrigin, direction, rayParams)
    return result == nil or result.Instance:IsDescendantOf(targetPart.Parent)
end

local function GetTarget()
    local target = nil
    local shortestDist = _G.FOVRadius
    
    for _, player in ipairs(Players:GetPlayers()) do
        if player == LocalPlayer then continue end
        if not player.Character then continue end
        
        local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
        if not humanoid or humanoid.Health <= 0 then continue end
        
        if _G.TeamCheck and player.Team == LocalPlayer.Team then continue end
        
        local part = player.Character:FindFirstChild(_G.LockPart)
        if not part then continue end
        
        if not IsVisible(part) then continue end
        
        local screenPos, onScreen = Camera:WorldToViewportPoint(part.Position)
        if not onScreen then continue end
        
        local mousePos = UserInputService:GetMouseLocation()
        local dist = (Vector2.new(screenPos.X, screenPos.Y) - mousePos).Magnitude
        
        if dist < shortestDist then
            shortestDist = dist
            target = part
        end
    end
    
    return target
end

-- ============================================
-- AIMLOCK LOOP (100%)
-- ============================================
RunService.RenderStepped:Connect(function()
    Circle.Position = UserInputService:GetMouseLocation()
    Circle.Radius = _G.FOVRadius
    Circle.Visible = _G.AimbotEnabled
    
    if not _G.AimbotEnabled then return end
    
    local target = GetTarget()
    if target then
        Camera.CFrame = CFrame.new(Camera.CFrame.Position, target.Position)
        Circle.Color = Color3.fromRGB(0, 255, 100)
    else
        Circle.Color = Color3.fromRGB(255, 50, 50)
    end
end)

-- ============================================
-- TRIGGERBOT LOOP
-- ============================================
local lastShot = 0

RunService.RenderStepped:Connect(function()
    if not _G.TriggerbotEnabled then return end
    if tick() - lastShot < _G.TriggerDelay then return end
    
    local target = GetTarget()
    if target then
        local virtualInput = game:GetService("VirtualInputManager")
        virtualInput:SendMouseButtonEvent(0, 0, 0, true, game, 1)
        task.wait(0.01)
        virtualInput:SendMouseButtonEvent(0, 0, 0, false, game, 1)
        lastShot = tick()
    end
end)

-- ============================================
-- ESP BOX SYSTEM
-- ============================================
local ESPCache = {}

local function CreateESP(player)
    if ESPCache[player] then return end
    
    local box = Drawing.new("Square")
    box.Thickness = 1.5
    box.Filled = false
    box.Transparency = 1
    box.Visible = false
    
    local nameTag = Drawing.new("Text")
    nameTag.Size = 14
    nameTag.Center = true
    nameTag.Outline = true
    nameTag.Visible = false
    
    local distTag = Drawing.new("Text")
    distTag.Size = 12
    distTag.Center = true
    distTag.Outline = true
    distTag.Visible = false
    
    ESPCache[player] = {Box = box, Name = nameTag, Dist = distTag}
end

local function RemoveESP(player)
    if ESPCache[player] then
        for _, v in pairs(ESPCache[player]) do
            v:Remove()
        end
        ESPCache[player] = nil
    end
end

for _, p in ipairs(Players:GetPlayers()) do
    if p ~= LocalPlayer then CreateESP(p) end
end

Players.PlayerAdded:Connect(function(p) CreateESP(p) end)
Players.PlayerRemoving:Connect(function(p) RemoveESP(p) end)

RunService.RenderStepped:Connect(function()
    for player, esp in pairs(ESPCache) do
        if not _G.ESPEnabled or not player.Character then
            esp.Box.Visible = false
            esp.Name.Visible = false
            esp.Dist.Visible = false
            continue
        end
        
        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
        local head = player.Character:FindFirstChild("Head")
        local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
        
        if not hrp or not head or not humanoid or humanoid.Health <= 0 then
            esp.Box.Visible = false
            esp.Name.Visible = false
            esp.Dist.Visible = false
            continue
        end
        
        local headPos, onScreen = Camera:WorldToViewportPoint(head.Position + Vector3.new(0, 1, 0))
        local hrpPos = Camera:WorldToViewportPoint(hrp.Position - Vector3.new(0, 3, 0))
        
        if onScreen then
            local height = math.abs(headPos.Y - hrpPos.Y)
            local width = height / 2
            
            local color = _G.ESPColor
            if _G.ESPTeamColor and player.Team then
                color = player.TeamColor.Color
            end
            
            esp.Box.Color = color
            esp.Box.Size = Vector2.new(width, height)
            esp.Box.Position = Vector2.new(headPos.X - width/2, headPos.Y)
            esp.Box.Visible = true
            
            if _G.ESPName then
                esp.Name.Text = player.Name
                esp.Name.Color = color
                esp.Name.Position = Vector2.new(headPos.X, headPos.Y - 20)
                esp.Name.Visible = true
            else
                esp.Name.Visible = false
            end
            
            if _G.ESPDistance then
                local dist = math.floor((Camera.CFrame.Position - hrp.Position).Magnitude)
                esp.Dist.Text = dist .. " studs"
                esp.Dist.Color = color
                esp.Dist.Position = Vector2.new(headPos.X, hrpPos.Y + 5)
                esp.Dist.Visible = true
            else
                esp.Dist.Visible = false
            end
        else
            esp.Box.Visible = false
            esp.Name.Visible = false
            esp.Dist.Visible = false
        end
    end
end)

-- ============================================
-- AUTO RELOAD
-- ============================================
RunService.Heartbeat:Connect(function()
    if not _G.AutoReloadEnabled then return end
    if not LocalPlayer.Character then return end
    
    local tool = LocalPlayer.Character:FindFirstChildOfClass("Tool")
    if tool then
        pcall(function()
            tool:Activate()
        end)
    end
end)

-- ============================================
-- NOTIFIKASI
-- ============================================
Rayfield:Notify({
   Title = "Fluxo PVP Full Pack",
   Content = "Script dimuat! Aktifkan fitur di menu.",
   Duration = 5,
})
