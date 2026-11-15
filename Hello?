loadstring('
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")

local Player = Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
local Humanoid = Character:WaitForChild("Humanoid")
local Camera = workspace.CurrentCamera

-- ========================
-- CONFIG
-- ========================
local Config = {
    AutoFarm = false,
    AutoFarmSpeed = 1.0,
    ESP = true,
    ESPColor = Color3.fromRGB(0, 255, 127),
    ESPHighlight = true,
    ShootMurd = false,
    AutoGetGun = false,
    CandyTarget = "Candy",
    MurdTarget = "Murd",
    GunName = "Gun",
    FarmRadius = 50,
    MurdRange = 300,
    ESPThickness = 2,
    ESPOpacity = 0.6
}

-- ========================
-- GUI SETUP
-- ========================
local function createGUI()
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "MM2AutoFarmGUI"
    ScreenGui.ResetOnSpawn = false
    ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
    ScreenGui.Parent = CoreGui

    local MainFrame = Instance.new("Frame")
    MainFrame.Name = "MainFrame"
    MainFrame.Size = UDim2.fromScale(0, 0.35)
    MainFrame.Position = UDim2.fromScale(0.05, 0.1)
    MainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 30)
    MainFrame.BorderSizePixel = 0
    MainFrame.Active = true
    MainFrame.Draggable = true
    MainFrame.Parent = ScreenGui

    local UICorner = Instance.new("UICorner")
    UICorner.CornerRadius = UDim.new(0, 10)
    UICorner.Parent = MainFrame

    local UIPadding = Instance.new("UIPadding")
    UIPadding.PaddingTop = UDim.new(0, 10)
    UIPadding.PaddingBottom = UDim.new(0, 10)
    UIPadding.PaddingLeft = UDim.new(0, 10)
    UIPadding.PaddingRight = UDim.new(0, 10)
    UIPadding.Parent = MainFrame

    local Title = Instance.new("TextLabel")
    Title.Name = "Title"
    Title.Size = UDim2.fromScale(1, 0.15)
    Title.Position = UDim2.fromScale(0, 0)
    Title.BackgroundTransparency = 1
    Title.Text = "MM2 Auto Farm & Combat"
    Title.TextColor3 = Color3.fromRGB(255, 255, 255)
    Title.TextSize = 18
    Title.Font = Enum.Font.GothamBold
    Title.Parent = MainFrame

    local ScrollFrame = Instance.new("ScrollingFrame")
    ScrollFrame.Name = "ScrollFrame"
    ScrollFrame.Size = UDim2.fromScale(1, 0.85)
    ScrollFrame.Position = UDim2.fromScale(0, 0.15)
    ScrollFrame.BackgroundTransparency = 1
    ScrollFrame.BorderSizePixel = 0
    ScrollFrame.ScrollBarThickness = 6
    ScrollFrame.ScrollBarImageColor3 = Color3.fromRGB(100, 100, 100)
    ScrollFrame.CanvasSize = UDim2.fromScale(0, 4)
    ScrollFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
    ScrollFrame.Parent = MainFrame

    local function createToggle(parent, name, label, initial, onChange)
        local Container = Instance.new("Frame")
        Container.Size = UDim2.fromScale(1, 0.08)
        Container.BackgroundTransparency = 1
        Container.LayoutOrder = 1
        Container.Parent = parent

        local Label = Instance.new("TextLabel")
        Label.Size = UDim2.fromScale(0.7, 1)
        Label.Position = UDim2.fromScale(0, 0)
        Label.BackgroundTransparency = 1
        Label.Text = label
        Label.TextColor3 = Color3.fromRGB(255, 255, 255)
        Label.TextSize = 14
        Label.Font = Enum.Font.Gotham
        Label.TextXAlignment = Enum.TextXAlignment.Left
        Label.Parent = Container

        local Toggle = Instance.new("TextButton")
        Toggle.Size = UDim2.fromScale(0.25, 0.8)
        Toggle.Position = UDim2.fromScale(0.75, 0.1)
        Toggle.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
        Toggle.Text = initial and "ON" or "OFF"
        Toggle.TextColor3 = Color3.fromRGB(255, 255, 255)
        Toggle.TextSize = 12
        Toggle.Font = Enum.Font.GothamBold
        Toggle.Parent = Container

        local UICornerToggle = Instance.new("UICorner")
        UICornerToggle.CornerRadius = UDim.new(0, 5)
        UICornerToggle.Parent = Toggle

        Toggle.MouseButton1Click:Connect(function()
            local newState = Toggle.Text == "OFF"
            Toggle.Text = newState and "ON" or "OFF"
            Toggle.BackgroundColor3 = newState and Color3.fromRGB(0, 150, 0) or Color3.fromRGB(60, 60, 60)
            if onChange then
                onChange(newState)
            end
        end)

        return Container, Toggle
    end

    local function createSlider(parent, name, label, min, max, initial, onChange)
        local Container = Instance.new("Frame")
        Container.Size = UDim2.fromScale(1, 0.12)
        Container.BackgroundTransparency = 1
        Container.LayoutOrder = 2
        Container.Parent = parent

        local Label = Instance.new("TextLabel")
        Label.Size = UDim2.fromScale(1, 0.5)
        Label.Position = UDim2.fromScale(0, 0)
        Label.BackgroundTransparency = 1
        Label.Text = label
        Label.TextColor3 = Color3.fromRGB(255, 255, 255)
        Label.TextSize = 14
        Label.Font = Enum.Font.Gotham
        Label.TextXAlignment = Enum.TextXAlignment.Left
        Label.Parent = Container

        local SliderBg = Instance.new("Frame")
        SliderBg.Size = UDim2.fromScale(1, 0.3)
        SliderBg.Position = UDim2.fromScale(0, 0.6)
        SliderBg.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
        SliderBg.BorderSizePixel = 0
        SliderBg.Parent = Container

        local UICornerSlider = Instance.new("UICorner")
        UICornerSlider.CornerRadius = UDim.new(0, 5)
        UICornerSlider.Parent = SliderBg

        local SliderFill = Instance.new("Frame")
        SliderFill.Size = UDim2.fromScale((initial - min) / (max - min), 1)
        SliderFill.Position = UDim2.fromScale(0, 0)
        SliderFill.BackgroundColor3 = Color3.fromRGB(0, 150, 0)
        SliderFill.BorderSizePixel = 0
        SliderFill.Parent = SliderBg

        local UICornerFill = Instance.new("UICorner")
        UICornerFill.CornerRadius = UDim.new(0, 5)
        UICornerFill.Parent = SliderFill

        local SliderValue = Instance.new("TextLabel")
        SliderValue.Size = UDim2.fromScale(0.3, 1)
        SliderValue.Position = UDim2.fromScale(0.7, 0)
        SliderValue.BackgroundTransparency = 1
        SliderValue.Text = tostring(initial)
        SliderValue.TextColor3 = Color3.fromRGB(255, 255, 255)
        SliderValue.TextSize = 12
        SliderValue.Font = Enum.Font.GothamBold
        SliderValue.TextXAlignment = Enum.TextXAlignment.Right
        SliderValue.Parent = Container

        local dragging = false
        local dragStart, startPos

        SliderBg.InputBegan:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                dragging = true
                dragStart = input.Position
                startPos = SliderFill.Size.X.Scale
            end
        end)

        SliderBg.InputEnded:Connect(function(input)
            if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
                dragging = false
            end
        end)

        SliderBg.InputChanged:Connect(function(input)
            if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
                local delta = input.Position - dragStart
                local newScale = math.clamp(startPos + delta.X / SliderBg.AbsoluteSize.X, 0, 1)
                SliderFill.Size = UDim2.fromScale(newScale, 1)
                local newValue = min + newScale * (max - min)
                SliderValue.Text = string.format("%.2f", newValue)
                if onChange then
                    onChange(newValue)
                end
            end
        end)

        return Container
    end

    -- Toggles
    createToggle(ScrollFrame, "AutoFarmToggle", "Candy Auto Farm", Config.AutoFarm, function(state)
        Config.AutoFarm = state
    end)

    createSlider(ScrollFrame, "SpeedSlider", "Auto Farm Speed", 0.1, 5.0, Config.AutoFarmSpeed, function(value)
        Config.AutoFarmSpeed = value
    end)

    createToggle(ScrollFrame, "ESPToggle", "ESP (Highlights)", Config.ESP, function(state)
        Config.ESP = state
    end)

    createToggle(ScrollFrame, "ShootMurdToggle", "Shoot Murd", Config.ShootMurd, function(state)
        Config.ShootMurd = state
    end)

    local function createButton(parent, label, onClick)
        local Btn = Instance.new("TextButton")
        Btn.Size = UDim2.fromScale(1, 0.08)
        Btn.BackgroundColor3 = Color3.fromRGB(0, 100, 200)
        Btn.Text = label
        Btn.TextColor3 = Color3.fromRGB(255, 255, 255)
        Btn.TextSize = 14
        Btn.Font = Enum.Font.GothamBold
        Btn.LayoutOrder = 3
        Btn.Parent = parent

        local UICornerBtn = Instance.new("UICorner")
        UICornerBtn.CornerRadius = UDim.new(0, 5)
        UICornerBtn.Parent = Btn

        Btn.MouseButton1Click:Connect(onClick)
        return Btn
    end

    createButton(ScrollFrame, "Get Gun (Instant)", function()
        print("Attempting to get gun...")
        local ReplicatedStorage = game:GetService("ReplicatedStorage")
        local Shared = ReplicatedStorage:FindFirstChild("Shared")
        local Guns = Shared and Shared:FindFirstChild("Guns")

        if Guns then
            for _, gunData in pairs(Guns:GetChildren()) do
                if typeof(gunData) == "ModuleScript" or typeof(gunData) == "Folder" then
                    local Character = Player.Character
                    if Character then
                        local Tools = Character:FindFirstChildOfClass("Tools")
                        if Tools then
                            print("Available guns: " .. table.concat(Guns:GetChildren(), ", "))
                        end
                    end
                end
            end
        end
    end)

    createButton(ScrollFrame, "Reset to Spawn", function()
        local SpawnLocation = workspace:FindFirstChild("SpawnLocation")
        if SpawnLocation then
            Character:PivotTo(SpawnLocation.CFrame)
            print("Reset to spawn location.")
        else
            print("SpawnLocation not found.")
        end
    end)

    return ScreenGui
end

local GUI = createGUI()

-- ========================
-- ESP SYSTEM
-- ========================
local HighlightManager = {}

function HighlightManager:Update()
    if not Config.ESP then
        return
    end

    local targets = {}
    for _, part in pairs(workspace:GetDescendants()) do
        if part:IsA("BasePart") then
            local name = part.Name
            if name:find(Config.CandyTarget, 1, true) or name:find(Config.MurdTarget, 1, true) then
                if not targets[name] then
                    local highlight = Instance.new("Highlight")
                    highlight.Name = "ESP_" .. name
                    highlight.Parent = part
                    highlight.FillColor = Config.ESPColor
                    highlight.FillTransparency = 1 - Config.ESPOpacity
                    highlight.OutlineColor = Config.ESPColor
                    highlight.OutlineTransparency = 1 - Config.ESPOpacity
                    highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
                    targets[name] = highlight
                end
            end
        end
    end

    for name, highlight in pairs(targets) do
        if not highlight.Parent then
            targets[name] = nil
        end
    end
end

-- ========================
-- AUTO FARM SYSTEM
-- ========================
local function getCandyPositions()
    local positions = {}
    for _, part in pairs(workspace:GetDescendants()) do
        if part:IsA("BasePart") and part.Name:find(Config.CandyTarget, 1, true) then
            table.insert(positions, part.Position)
        end
    end
    return positions
end

local function autoFarm()
    if not Config.AutoFarm then
        return
    end

    local positions = getCandyPositions()
    if #positions == 0 then
        return
    end

    local closestPos = positions[1]
    local closestDist = (HumanoidRootPart.Position - closestPos).Magnitude

    for _, pos in ipairs(positions) do
        local dist = (HumanoidRootPart.Position - pos).Magnitude
        if dist < closestDist then
            closestDist = dist
            closestPos = pos
        end
    end

    if closestDist < Config.FarmRadius then
        local tool = Character:FindFirstChildOfClass("Tool")
        if tool then
            if tool.Name:find(Config.CandyTarget, 1, true) then
                tool:Activate()
            end
        end
    else
        local dir = (closestPos - HumanoidRootPart.Position).Unit
        Humanoid:MoveDirection(dir * Config.AutoFarmSpeed)
    end
end

-- ========================
-- SHOOT MURD SYSTEM
-- ========================
local function shootMurd()
    if not Config.ShootMurd then
        return
    end

    local tool = Character:FindFirstChildOfClass("Tool")
    if not tool then
        return
    end

    local closestMurd = nil
    local closestDist = Config.MurdRange

    for _, part in pairs(workspace:GetDescendants()) do
        if part:IsA("BasePart") and part.Name:find(Config.MurdTarget, 1, true) then
            local dist = (HumanoidRootPart.Position - part.Position).Magnitude
            if dist < closestDist then
                closestDist = dist
                closestMurd = part
            end
        end
    end

    if closestMurd then
        local aimPoint = closestMurd.Position + Vector3.new(0, 1, 0)
        local lookAt = CFrame.lookAt(HumanoidRootPart.Position, aimPoint)
        HumanoidRootPart.CFrame = lookAt
        tool:Activate()
    end
end

-- ========================
-- MAIN LOOP
-- ========================
local lastUpdate = 0

RunService.Heartbeat:Connect(function(dt)
    local now = tick()
    if now - lastUpdate < 0.1 / Config.AutoFarmSpeed then
        return
    end
    lastUpdate = now

    if Character and Humanoid and HumanoidRootPart then
        HighlightManager:Update()

        if Config.AutoFarm then
            autoFarm()
        end

        if Config.ShootMurd then
            shootMurd()
        end
    end
end)

Player.CharacterAdded:Connect(function(newChar)
    Character = newChar
    HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
    Humanoid = Character:WaitForChild("Humanoid")
end)

print("MM2 Auto Farm & Combat GUI loaded successfully!")
')()
