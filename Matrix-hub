-- Services
local Players = game:GetService("Players")
local CoreGui = game:GetService("CoreGui")
local player = Players.LocalPlayer

-- Create ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "MatrixHub"
screenGui.ResetOnSpawn = false
screenGui.Parent = CoreGui

-- === DRAG FUNCTION ===
local function makeDraggable(frame)
    local dragging, dragInput, dragStart, startPos

    frame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = frame.Position

            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)

    frame.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement then
            dragInput = input
        end
    end)

    game:GetService("UserInputService").InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            frame.Position = UDim2.new(
                startPos.X.Scale,
                startPos.X.Offset + delta.X,
                startPos.Y.Scale,
                startPos.Y.Offset + delta.Y
            )
        end
    end)
end

-- === CATEGORY CREATOR ===
local function createCategory(title, position)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0, 300, 0, 400)
    frame.Position = position
    frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    frame.BorderSizePixel = 0
    frame.Parent = screenGui

    local titleLabel = Instance.new("TextLabel")
    titleLabel.Size = UDim2.new(1, 0, 0, 50)
    titleLabel.Text = title
    titleLabel.TextScaled = true
    titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    titleLabel.BackgroundColor3 = Color3.fromRGB(0, 100, 200)
    titleLabel.Parent = frame

    makeDraggable(frame)
    return frame
end

-- === BUTTON CREATOR ===
local function createButton(frame, text, position)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0, 280, 0, 50)
    button.Position = position
    button.Text = text
    button.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Parent = frame
    return button
end

-- === SLIDER CREATOR ===
local function createSlider(frame, text, position, minValue, maxValue)
    local sliderFrame = Instance.new("Frame")
    sliderFrame.Size = UDim2.new(0, 280, 0, 50)
    sliderFrame.Position = position
    sliderFrame.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
    sliderFrame.Parent = frame

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1, -10, 0, 25)
    label.Position = UDim2.new(0, 5, 0, 0)
    label.Text = text .. ": " .. minValue
    label.TextColor3 = Color3.fromRGB(255, 255, 255)
    label.BackgroundTransparency = 1
    label.Parent = sliderFrame

    local slideButton = Instance.new("TextButton")
    slideButton.Size = UDim2.new(1, -10, 0, 10)
    slideButton.Position = UDim2.new(0, 5, 0, 30)
    slideButton.Text = ""
    slideButton.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
    slideButton.Parent = sliderFrame

    slideButton.MouseButton1Down:Connect(function()
        local mouse = player:GetMouse()
        local percent = math.clamp(
            (mouse.X - slideButton.AbsolutePosition.X) / slideButton.AbsoluteSize.X,
            0,
            1
        )
        local value = math.floor(minValue + (maxValue - minValue) * percent)
        label.Text = text .. ": " .. value
    end)

    return label, slideButton
end

-- === TOGGLE CREATOR ===
local function createToggle(frame, text, position)
    local toggleButton = Instance.new("TextButton")
    toggleButton.Size = UDim2.new(0, 280, 0, 50)
    toggleButton.Position = position
    toggleButton.Text = text .. ": OFF"
    toggleButton.BackgroundColor3 = Color3.fromRGB(150, 50, 50)
    toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    toggleButton.Parent = frame

    local isActive = false

    toggleButton.MouseButton1Click:Connect(function()
        isActive = not isActive
        toggleButton.Text = text .. ": " .. (isActive and "ON" or "OFF")
        toggleButton.BackgroundColor3 =
            isActive and Color3.fromRGB(50, 150, 50)
            or Color3.fromRGB(150, 50, 50)
    end)

    return toggleButton
end

-- === CATCHING CATEGORY ===
local catchingFrame = createCategory("Catching", UDim2.new(0, 50, 0, 50))
local magnetToggle = createToggle(catchingFrame, "Ball Magnet", UDim2.new(0, 10, 0, 60))
local magnetPowerLabel, magnetPowerSlider =
    createSlider(catchingFrame, "Magnet Power", UDim2.new(0, 10, 0, 120), 5, 100)

-- === PLAYER CATEGORY ===
local playerFrame = createCategory("Player", UDim2.new(0, 400, 0, 50))
local speedLabel, speedSlider =
    createSlider(playerFrame, "Player Speed", UDim2.new(0, 10, 0, 60), 16, 100)

-- Speed functionality
speedSlider.MouseButton1Up:Connect(function()
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        local speedValue = tonumber(speedLabel.Text:match("%d+")) or 16
        humanoid.WalkSpeed = speedValue
    end
end)

-- Magnet toggle logic placeholder
magnetToggle.MouseButton1Click:Connect(function()
    local power = tonumber(magnetPowerLabel.Text:match("%d+")) or 5
    print("Ball Magnet toggled | Power:", power)
end)

print("Matrix Hub GUI Created Successfully")
