-- 💥 STAND 💥 GUI Final Version
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local CoreGui = game:GetService("CoreGui")

-- Cleanup
pcall(function()
	if CoreGui:FindFirstChild("STAND_UI") then
		CoreGui.STAND_UI:Destroy()
	end
end)

-- GUI Setup
local gui = Instance.new("ScreenGui", CoreGui)
gui.Name = "STAND_UI"
gui.ResetOnSpawn = false

-- Toggle Button
local toggleBtn = Instance.new("TextButton", gui)
toggleBtn.Size = UDim2.new(0, 60, 0, 60)
toggleBtn.Position = UDim2.new(0, 10, 0, 10)
toggleBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
toggleBtn.Text = "💥 STAND 💥"
toggleBtn.TextColor3 = Color3.fromRGB(255, 140, 0)
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.TextScaled = true
toggleBtn.Draggable = true

-- Main Frame
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.new(0, 500, 0, 300)
frame.Position = UDim2.new(0.5, -250, 0.5, -150)
frame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
frame.Visible = true
frame.Active = true
frame.Draggable = true

-- Border Around Frame
local stroke = Instance.new("UIStroke", frame)
stroke.Color = Color3.fromRGB(255, 140, 0)
stroke.Thickness = 3
stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

-- Title Label
local title = Instance.new("TextLabel", frame)
title.Size = UDim2.new(1, 0, 0, 40)
title.Position = UDim2.new(0, 0, 0, 0)
title.Text = "💥 STAND 💥"
title.TextColor3 = Color3.fromRGB(255, 140, 0)
title.Font = Enum.Font.GothamBlack
title.TextScaled = true
title.BackgroundColor3 = Color3.fromRGB(35, 35, 35)

-- Button Grid Layout
local grid = Instance.new("UIGridLayout", frame)
grid.CellSize = UDim2.new(0, 150, 0, 50)
grid.CellPadding = UDim2.new(0, 10, 0, 10)
grid.FillDirection = Enum.FillDirection.Horizontal
grid.HorizontalAlignment = Enum.HorizontalAlignment.Center
grid.VerticalAlignment = Enum.VerticalAlignment.Center
grid.SortOrder = Enum.SortOrder.LayoutOrder

-- Function to Create Buttons
local function createButton(text, callback)
	local btn = Instance.new("TextButton")
	btn.Text = text
	btn.Font = Enum.Font.GothamBold
	btn.TextScaled = true
	btn.TextColor3 = Color3.new(1, 1, 1)
	btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	btn.Size = UDim2.new(0, 150, 0, 50)
	btn.MouseButton1Click:Connect(callback)
	btn.Parent = frame
end

-- 1. Instant Respawn
createButton("Instant Respawn", function()
	local function InstantRespawn()
		spawn(function()
			repeat RunService.Stepped:Wait() until LocalPlayer.Character:FindFirstChildWhichIsA("Humanoid")
			LocalPlayer.Character:FindFirstChildWhichIsA("Humanoid").Died:Connect(function()
				ReplicatedStorage:WaitForChild("Guide"):FireServer()
			end)
		end)
	end
	LocalPlayer.CharacterAdded:Connect(function()
		LocalPlayer.Character:WaitForChild("HumanoidRootPart")
		RunService.Stepped:Wait()
		InstantRespawn()
	end)
	InstantRespawn()
end)

-- 2. Grab Tools
createButton("Grab Tools", function()
	local excludedBases = { "Insanity", "Giant", "Dark", "Spike", "Web", "Strong" }
	local allowedBases = { "Stone", "Magic", "Storm" }
	local function isBaseAllowed(baseName)
		return table.find(allowedBases, baseName) or not table.find(excludedBases, baseName)
	end
	local function CollectTools()
		local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
		local Root = character:FindFirstChild("HumanoidRootPart")
		if not Root then return end
		for _, v in ipairs(workspace.Tycoons:GetDescendants()) do
			if v:IsA("TouchTransmitter") and v.Parent.Parent.Name:find("GearGiver1") then
				local baseName = v.Parent.Parent.Parent and v.Parent.Parent.Parent.Name
				if baseName and isBaseAllowed(baseName) then
					firetouchinterest(Root, v.Parent, 0)
					firetouchinterest(Root, v.Parent, 1)
				end
			end
		end
	end
	task.spawn(function()
		while task.wait(0.5) do
			local success, err = pcall(CollectTools)
			if not success then warn("Grab Tools Error: ", err) end
		end
	end)
	LocalPlayer.CharacterAdded:Connect(function()
		task.wait(0.5)
		local success, err = pcall(CollectTools)
		if not success then warn("Error on Respawn Tool Grab:", err) end
	end)
end)

-- 3. Speed + Jump + Inf Jump
createButton("Speed + Jump + Inf Jump", function()
	local SPEED = 70
	local JUMP = 150
	local function applyStats(character)
		local humanoid = character:WaitForChild("Humanoid", 5)
		if humanoid then
			humanoid.WalkSpeed = SPEED
			humanoid.JumpPower = JUMP
		end
	end
	if LocalPlayer.Character then applyStats(LocalPlayer.Character) end
	LocalPlayer.CharacterAdded:Connect(function(char)
		char:WaitForChild("HumanoidRootPart", 5)
		task.wait(0.5)
		applyStats(char)
	end)
	local infJumpEnabled = true
	UserInputService.JumpRequest:Connect(function()
		if infJumpEnabled then
			local char = LocalPlayer.Character
			if char and char:FindFirstChild("HumanoidRootPart") then
				char.HumanoidRootPart.Velocity = Vector3.new(0, 50, 0)
			end
		end
	end)
end)

-- 4. Ghost Tool (loadstring)
createButton("Ghost Tool", function()
	loadstring(game:HttpGet("https://raw.githubusercontent.com/Ghostly9999/Use-tools-no-movvv/refs/heads/main/README.md"))()
end)

-- 5. Hook Wait
createButton("Hook Wait", function()
	local v0=string.char;local v1=string.byte;local v2=string.sub;local v3=bit32 or bit ;local v4=v3.bxor;
	local v5=table.concat;local v6=table.insert;
	local function v7(v10,v11)
		local v12={};for v14=1,#v10 do
			v6(v12,v0(v4(v1(v2(v10,v14,v14+1)),v1(v2(v11,1+(v14%#v11),1+(v14%#v11)+1)))%256))
		end return v5(v12)
	end
	local v8=game:GetService(v7("\227\214\213\22\227\169\209\23\210\198","\126\177\163\187\69\134\219\167")).PostSimulation;
	hookfunction(wait,function() return v8:Wait() end)
	hookfunction(task.wait,function() return v8:Wait() end)
end)

-- 6. Float Tools
createButton("Float Tools", function()
	local offsets = {
		["Axe"] = Vector3.new(13, -10, -4),
		["Staff"] = Vector3.new(2, 10, -16),
		["Energy Sword"] = Vector3.new(-13, -10, -4),
	}
	local function FloatTools()
		local char = LocalPlayer.Character
		if char then
			for _, tool in pairs(char:GetChildren()) do
				if tool:IsA("Tool") and offsets[tool.Name] then
					tool.Grip = CFrame.new(offsets[tool.Name])
				end
			end
		end
	end
	FloatTools()
	LocalPlayer.CharacterAdded:Connect(function()
		wait(1)
		FloatTools()
	end)
end)

-- Toggle GUI visibility
toggleBtn.MouseButton1Click:Connect(function()
	frame.Visible = not frame.Visible
end)
