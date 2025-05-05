local function showNotification()
  
    local player = game.Players.LocalPlayer
    local screenGui = Instance.new("ScreenGui")
    screenGui.Parent = player.PlayerGui
    
    local notification = Instance.new("TextLabel")
    notification.Parent = screenGui
    notification.Text = "Script Loaded, Press G to Activate"
    notification.Size = UDim2.new(0, 300, 0, 50)
    notification.Position = UDim2.new(1, -310, 1, -60)
    notification.BackgroundTransparency = 1 -- Fundo transparente
    notification.TextColor3 = Color3.fromRGB(255, 255, 255)
    notification.TextSize = 20
    notification.TextScaled = true
    notification.AnchorPoint = Vector2.new(1, 1)

    notification.Font = Enum.Font.ComicSans -- Fonte Comic Sans

    notification.TextTransparency = 1
    notification.BackgroundTransparency = 1
    local tweenService = game:GetService("TweenService")
    local fadeInTween = tweenService:Create(notification, TweenInfo.new(0.5), {TextTransparency = 0, BackgroundTransparency = 0})
    fadeInTween:Play()
    fadeInTween.Completed:Wait()

    local userInputService = game:GetService("UserInputService")

    userInputService.InputBegan:Connect(function(input, gameProcessedEvent)
        if not gameProcessedEvent and input.KeyCode == Enum.KeyCode.G then
      
            local fadeOutTween = tweenService:Create(notification, TweenInfo.new(1), {TextTransparency = 1, BackgroundTransparency = 1})
            fadeOutTween:Play()

            fadeOutTween.Completed:Wait()

            screenGui:Destroy()
        end
    end)
end

showNotification()


local UIS = game:GetService("UserInputService")
local player = game.Players.LocalPlayer
local speedOn = false

local function toggleSpeed()
	local character = player.Character or player.CharacterAdded:Wait()
	local humanoid = character:WaitForChild("Humanoid")
	
	if speedOn then
		humanoid.WalkSpeed = 16
	else
		humanoid.WalkSpeed = 50
	end
	
	speedOn = not speedOn
end

UIS.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.G then
		toggleSpeed()
	end
end)

local UIS = game:GetService("UserInputService")
local player = game.Players.LocalPlayer
local jumping = false

UIS.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.G then
		jumping = not jumping
	end
end)

UIS.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.Space and jumping then
		local character = player.Character or player.CharacterAdded:Wait()
		local humanoidRoot = character:FindFirstChild("HumanoidRootPart")
		if humanoidRoot then
			humanoidRoot.Velocity = Vector3.new(0, 75, 0)
		end
	end
end)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")

local player = Players.LocalPlayer
local noclip = false

UIS.InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.G then
		noclip = not noclip
	end
end)

RunService.Stepped:Connect(function()
	if noclip and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
		for _, part in ipairs(player.Character:GetDescendants()) do
			if part:IsA("BasePart") and part.CanCollide then
				part.CanCollide = false
			end
		end
	end
end)
