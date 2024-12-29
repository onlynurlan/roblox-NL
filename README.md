# roblox-NL script
# made by nursultan 
local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local toggleKey = Enum.KeyCode.Insert -- начальный бинд тоесть insert (если скрипт не работает уберите все что написано на русском)
local isMenuOpen = true
local flyEnabled = false
local noclipEnabled = false
local walkSpeed = 16
local jumpPower = 50

local titleSequence = { 
    "N", "N3", "N3\\", "N3\\/4", "N3\\/4/_", "N3\\/4/_0", "N3\\/4/_0SE"
}


local screenGui = Instance.new("ScreenGui", playerGui)
screenGui.Name = "Neverlose.cc (Beta)"
screenGui.ResetOnSpawn = false

local dynamicTitle = Instance.new("TextLabel")
dynamicTitle.Parent = screenGui
dynamicTitle.Size = UDim2.new(0.15, 0, 0.05, 0)
dynamicTitle.Position = UDim2.new(0.01, 0, 0.01, 0)
dynamicTitle.Text = titleSequence[1]
dynamicTitle.TextColor3 = Color3.fromRGB(0, 200, 255)
dynamicTitle.BackgroundTransparency = 1
dynamicTitle.Font = Enum.Font.GothamBlack
dynamicTitle.TextSize = 24
dynamicTitle.TextXAlignment = Enum.TextXAlignment.Left

local currentIndex = 1
task.spawn(function()
    while true do
        dynamicTitle.Text = titleSequence[currentIndex]
        currentIndex = currentIndex % #titleSequence + 1
        task.wait(1)
    end
end)

local mainFrame = Instance.new("Frame")
mainFrame.Parent = screenGui
mainFrame.Size = UDim2.new(0.35, 0, 0.5, 0)
mainFrame.Position = UDim2.new(0.325, 0, 0.25, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
mainFrame.BorderSizePixel = 0
mainFrame.Visible = isMenuOpen
mainFrame.Active = true
mainFrame.Draggable = true


local menuTitle = Instance.new("TextLabel")
menuTitle.Parent = mainFrame
menuTitle.Size = UDim2.new(1, 0, 0.1, 0)
menuTitle.Text = "Neverlose.cc (Beta)"
menuTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
menuTitle.Font = Enum.Font.GothamBlack
menuTitle.TextSize = 18
menuTitle.BackgroundColor3 = Color3.fromRGB(20, 20, 20)


local sidebar = Instance.new("Frame")
sidebar.Parent = mainFrame
sidebar.Size = UDim2.new(0.25, 0, 0.9, 0)
sidebar.Position = UDim2.new(0, 0, 0.1, 0)
sidebar.BackgroundColor3 = Color3.fromRGB(20, 20, 20)

local function createSidebarButton(name, position)
    local button = Instance.new("TextButton")
    button.Parent = sidebar
    button.Size = UDim2.new(1, 0, 0.15, 0)
    button.Position = position
    button.Text = name
    button.TextColor3 = Color3.fromRGB(200, 200, 200)
    button.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    button.Font = Enum.Font.Gotham
    button.TextSize = 16
    button.BorderSizePixel = 0
    return button
end

local movementTab = createSidebarButton("Movement", UDim2.new(0, 0, 0, 0))
local settingsTab = createSidebarButton("Settings", UDim2.new(0, 0, 0.2, 0))


local contentFrame = Instance.new("Frame")
contentFrame.Parent = mainFrame
contentFrame.Size = UDim2.new(0.75, 0, 0.9, 0)
contentFrame.Position = UDim2.new(0.25, 0, 0.1, 0)
contentFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)

local function createContentFrame()
    local frame = Instance.new("Frame")
    frame.Parent = contentFrame
    frame.Size = UDim2.new(1, 0, 1, 0)
    frame.BackgroundTransparency = 1
    frame.Visible = false
    return frame
end

local movementFrame = createContentFrame()
local settingsFrame = createContentFrame()


movementTab.MouseButton1Click:Connect(function()
    movementFrame.Visible = true
    settingsFrame.Visible = false
end)

settingsTab.MouseButton1Click:Connect(function()
    movementFrame.Visible = false
    settingsFrame.Visible = true
end)


local function createSlider(parent, label, min, max, default, position, callback)
    local frame = Instance.new("Frame")
    frame.Parent = parent
    frame.Size = UDim2.new(0.8, 0, 0.1, 0)
    frame.Position = position
    frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)

    local sliderLabel = Instance.new("TextLabel")
    sliderLabel.Parent = frame
    sliderLabel.Size = UDim2.new(0.5, 0, 1, 0)
    sliderLabel.Text = label
    sliderLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    sliderLabel.Font = Enum.Font.Gotham
    sliderLabel.TextSize = 14
    sliderLabel.BackgroundTransparency = 1

    local sliderInput = Instance.new("TextBox")
    sliderInput.Parent = frame
    sliderInput.Size = UDim2.new(0.5, 0, 1, 0)
    sliderInput.Position = UDim2.new(0.5, 0, 0, 0)
    sliderInput.Text = tostring(default)
    sliderInput.Font = Enum.Font.Gotham
    sliderInput.TextSize = 14
    sliderInput.TextColor3 = Color3.fromRGB(255, 255, 255)
    sliderInput.BackgroundColor3 = Color3.fromRGB(30, 30, 30)

    sliderInput.FocusLost:Connect(function()
        local value = tonumber(sliderInput.Text)
        if value and value >= min and value <= max then
            callback(value)
        else
            sliderInput.Text = tostring(default)
        end
    end)
end

createSlider(movementFrame, "WalkSpeed", 16, 9999999999999999999999999999999999, walkSpeed, UDim2.new(0.1, 0, 0.1, 0), function(value)
    walkSpeed = value
    player.Character.Humanoid.WalkSpeed = walkSpeed
end)

createSlider(movementFrame, "JumpPower", 50, 9999999999999999999999999999999999999999, jumpPower, UDim2.new(0.1, 0, 0.25, 0), function(value)
    jumpPower = value
    player.Character.Humanoid.JumpPower = jumpPower
end)


local toggleKeyText = Instance.new("TextLabel")
toggleKeyText.Parent = settingsFrame
toggleKeyText.Size = UDim2.new(0.8, 0, 0.1, 0)
toggleKeyText.Position = UDim2.new(0.1, 0, 0.1, 0)
toggleKeyText.Text = "Toggle Menu Key: " .. toggleKey.Name
toggleKeyText.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleKeyText.Font = Enum.Font.Gotham
toggleKeyText.TextSize = 16
toggleKeyText.BackgroundTransparency = 1

local changeKeyButton = Instance.new("TextButton")
changeKeyButton.Parent = settingsFrame
changeKeyButton.Size = UDim2.new(0.8, 0, 0.1, 0)
changeKeyButton.Position = UDim2.new(0.1, 0, 0.25, 0)
changeKeyButton.Text = "Click for change menu bind"
changeKeyButton.Font = Enum.Font.Gotham
changeKeyButton.TextSize = 16
changeKeyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
changeKeyButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)

changeKeyButton.MouseButton1Click:Connect(function()
    toggleKeyText.Text = "Press New Key"
    local connection
    connection = game:GetService("UserInputService").InputBegan:Connect(function(input)
        if input.KeyCode ~= Enum.KeyCode.Unknown then
            toggleKey = input.KeyCode
            toggleKeyText.Text = "Toggle Menu Key: " .. toggleKey.Name
            connection:Disconnect()
        end
    end)
end)

game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == toggleKey then
        isMenuOpen = not isMenuOpen
        mainFrame.Visible = isMenuOpen
    end
end)
