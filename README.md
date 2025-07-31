local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "xredxGui"
gui.ResetOnSpawn = false

-- Main Frame
local frame = Instance.new("Frame", gui)
frame.Name = "MainPage"
frame.Size = UDim2.new(0, 250, 0, 180)
frame.Position = UDim2.new(0.5, -125, 0.5, -90)
frame.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
frame.BorderSizePixel = 0
frame.Active = true
frame.Draggable = true

-- UICorner
local corner = Instance.new("UICorner", frame)
corner.CornerRadius = UDim.new(0, 20)

-- Title
local title = Instance.new("TextLabel", frame)
title.Text = "99 xredx"
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundTransparency = 1
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.Font = Enum.Font.GothamBold
title.TextScaled = true

-- Speed Button
local speedButton = Instance.new("TextButton", frame)
speedButton.Text = "Speed (10-100)"
speedButton.Size = UDim2.new(0, 200, 0, 40)
speedButton.Position = UDim2.new(0.5, -100, 0, 40)
speedButton.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
speedButton.Font = Enum.Font.GothamBold
speedButton.TextScaled = true
speedButton.TextColor3 = Color3.new(1, 1, 1)

local speedCorner = Instance.new("UICorner", speedButton)
speedCorner.CornerRadius = UDim.new(1, 0)

-- Fly Button
local flyButton = Instance.new("TextButton", frame)
flyButton.Text = "Fly"
flyButton.Size = UDim2.new(0, 200, 0, 40)
flyButton.Position = UDim2.new(0.5, -100, 0, 90)
flyButton.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
flyButton.Font = Enum.Font.GothamBold
flyButton.TextScaled = true
flyButton.TextColor3 = Color3.new(1, 1, 1)

local flyCorner = Instance.new("UICorner", flyButton)
flyCorner.CornerRadius = UDim.new(1, 0)

-- Close Button
local closeButton = Instance.new("TextButton", frame)
closeButton.Text = "X"
closeButton.Size = UDim2.new(0, 30, 0, 30)
closeButton.Position = UDim2.new(1, -35, 0, 5)
closeButton.BackgroundColor3 = Color3.fromRGB(120, 0, 0)
closeButton.Font = Enum.Font.GothamBold
closeButton.TextScaled = true
closeButton.TextColor3 = Color3.new(1, 1, 1)

local closeCorner = Instance.new("UICorner", closeButton)
closeCorner.CornerRadius = UDim.new(1, 0)

-- Close Function
closeButton.MouseButton1Click:Connect(function()
	gui:Destroy()
end)

-- Speed Script
speedButton.MouseButton1Click:Connect(function()
	local input = tonumber(game:GetService("StarterGui"):SetCore("ChatMakeSystemMessage", {
		Text = "Type speed (10-100) in chat",
		Color = Color3.new(1,1,1)
	}))
	
	player.Chatted:Connect(function(msg)
		local spd = tonumber(msg)
		if spd and spd >= 10 and spd <= 100 then
			if player.Character and player.Character:FindFirstChild("Humanoid") then
				player.Character.Humanoid.WalkSpeed = spd
			end
		end
	end)
end)

-- Fly Script
local flying = false
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local function startFly()
	local bodyGyro = Instance.new("BodyGyro")
	local bodyVelocity = Instance.new("BodyVelocity")
	local hrp = player.Character:WaitForChild("HumanoidRootPart")
	bodyGyro.P = 9e4
	bodyGyro.maxTorque = Vector3.new(9e9, 9e9, 9e9)
	bodyGyro.cframe = hrp.CFrame
	bodyGyro.Parent = hrp

	bodyVelocity.velocity = Vector3.new(0,0,0)
	bodyVelocity.maxForce = Vector3.new(9e9, 9e9, 9e9)
	bodyVelocity.Parent = hrp

	local flying = true
	RunService.RenderStepped:Connect(function()
		if flying then
			local cf = workspace.CurrentCamera.CFrame
			bodyVelocity.velocity = cf.lookVector * 60
			bodyGyro.CFrame = cf
		end
	end)
end

flyButton.MouseButton1Click:Connect(function()
	startFly()
end)
