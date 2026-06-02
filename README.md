local P = game:GetService("Players")
local Lp = P.LocalPlayer
local Rs = game:GetService("RunService")
local Uis = game:GetService("UserInputService")
local Ts = game:GetService("TweenService")

local antibatactive = false
local antibatconn = nil

local function startantibat()
	local char = Lp.Character
	if not char then return end
	local root = char:FindFirstChild("HumanoidRootPart")
	if not root then return end
	if antibatconn then antibatconn:Disconnect() end
	antibatconn = Rs.Heartbeat:Connect(function()
		if not root or not root.Parent then return end
		local orig = root.Velocity
		root.Velocity = Vector3.new(1000, root.Velocity.Y, 1000)
		Rs.RenderStepped:Wait()
		root.Velocity = orig
	end)
end

local function stopantibat()
	if antibatconn then antibatconn:Disconnect() antibatconn = nil end
end

Lp.CharacterAdded:Connect(function()
	if antibatactive then stopantibat() task.wait(0.2) startantibat() end
end)

local sg = Instance.new("ScreenGui")
sg.Name = "GhostAntiBat"
sg.ResetOnSpawn = false
sg.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
sg.Parent = Lp:WaitForChild("PlayerGui")

local main = Instance.new("Frame")
main.Name = "Main"
main.Size = UDim2.new(0, 220, 0, 100)
main.Position = UDim2.new(0.5, -110, 0.2, 0)
main.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
main.BorderSizePixel = 0
main.Parent = sg

local maincorner = Instance.new("UICorner")
maincorner.CornerRadius = UDim.new(0, 12)
maincorner.Parent = main

local mainstroke = Instance.new("UIStroke")
mainstroke.Color = Color3.fromRGB(80, 0, 0)
mainstroke.Thickness = 1
mainstroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
mainstroke.Parent = main

local header = Instance.new("Frame")
header.Name = "Header"
header.Size = UDim2.new(1, 0, 0, 32)
header.Position = UDim2.new(0, 0, 0, 0)
header.BackgroundTransparency = 1
header.Parent = main

local headertitle = Instance.new("TextLabel")
headertitle.Size = UDim2.new(1, 0, 1, 0)
headertitle.Position = UDim2.new(0, 0, 0, 0)
headertitle.BackgroundTransparency = 1
headertitle.Text = "Ghost Anti-Bat"
headertitle.TextColor3 = Color3.fromRGB(255, 0, 0)
headertitle.Font = Enum.Font.GothamBold
headertitle.TextSize = 18
headertitle.Parent = header

local card = Instance.new("Frame")
card.Size = UDim2.new(1, -20, 0, 52)
card.Position = UDim2.new(0, 10, 0, 38)
card.BackgroundColor3 = Color3.fromRGB(20, 0, 0)
card.BorderSizePixel = 0
card.Parent = main

local cardcorner = Instance.new("UICorner")
cardcorner.CornerRadius = UDim.new(0, 10)
cardcorner.Parent = card

local cardstroke = Instance.new("UIStroke")
cardstroke.Color = Color3.fromRGB(80, 0, 0)
cardstroke.Thickness = 1
cardstroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
cardstroke.Parent = card

local label = Instance.new("TextLabel")
label.Size = UDim2.new(0.6, 0, 0, 22)
label.Position = UDim2.new(0, 14, 0, 8)
label.BackgroundTransparency = 1
label.Text = "PROTECTION"
label.TextColor3 = Color3.fromRGB(255, 255, 255)
label.Font = Enum.Font.GothamBold
label.TextSize = 14
label.TextXAlignment = Enum.TextXAlignment.Left
label.Parent = card

local status = Instance.new("TextLabel")
status.Size = UDim2.new(0.6, 0, 0, 14)
status.Position = UDim2.new(0, 14, 0, 28)
status.BackgroundTransparency = 1
status.Text = "DISABLED"
status.TextColor3 = Color3.fromRGB(150, 0, 0)
status.Font = Enum.Font.GothamBold
status.TextSize = 10
status.TextXAlignment = Enum.TextXAlignment.Left
status.Parent = card

local pillbg = Instance.new("Frame")
pillbg.Size = UDim2.new(0, 40, 0, 22)
pillbg.Position = UDim2.new(1, -54, 0.5, -11)
pillbg.BackgroundColor3 = Color3.fromRGB(40, 0, 0)
pillbg.BorderSizePixel = 0
pillbg.Parent = card

local pillcorner = Instance.new("UICorner")
pillcorner.CornerRadius = UDim.new(1, 0)
pillcorner.Parent = pillbg

local knob = Instance.new("Frame")
knob.Size = UDim2.new(0, 14, 0, 14)
knob.Position = UDim2.new(0, 4, 0.5, -7)
knob.BackgroundColor3 = Color3.fromRGB(80, 85, 95)
knob.BorderSizePixel = 0
knob.Parent = pillbg

local knobcorner = Instance.new("UICorner")
knobcorner.CornerRadius = UDim.new(1, 0)
knobcorner.Parent = knob

local btn = Instance.new("TextButton")
btn.Size = UDim2.new(1, 0, 1, 0)
btn.BackgroundTransparency = 1
btn.Text = ""
btn.Parent = card

local tweeninfo = TweenInfo.new(0.15, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)

local function setstate(active)
	if active then
		Ts:Create(cardstroke, tweeninfo, {Color = Color3.fromRGB(255, 0, 0)}):Play()
		status.Text = "ENABLED"
		Ts:Create(status, tweeninfo, {TextColor3 = Color3.fromRGB(255, 0, 0)}):Play()
		Ts:Create(pillbg, tweeninfo, {BackgroundColor3 = Color3.fromRGB(30, 0, 0)}):Play()
		Ts:Create(knob, tweeninfo, {BackgroundColor3 = Color3.fromRGB(255, 0, 0)}):Play()
		Ts:Create(knob, tweeninfo, {Position = UDim2.new(0, 22, 0.5, -7)}):Play()
	else
		Ts:Create(cardstroke, tweeninfo, {Color = Color3.fromRGB(80, 0, 0)}):Play()
		status.Text = "DISABLED"
		Ts:Create(status, tweeninfo, {TextColor3 = Color3.fromRGB(150, 0, 0)}):Play()
		Ts:Create(pillbg, tweeninfo, {BackgroundColor3 = Color3.fromRGB(40, 0, 0)}):Play()
		Ts:Create(knob, tweeninfo, {BackgroundColor3 = Color3.fromRGB(80, 85, 95)}):Play()
		Ts:Create(knob, tweeninfo, {Position = UDim2.new(0, 4, 0.5, -7)}):Play()
	end
end

setstate(false)

btn.MouseButton1Click:Connect(function()
	antibatactive = not antibatactive
	setstate(antibatactive)
	if antibatactive then startantibat() else stopantibat() end
end)

local dragging, draginput, dragstart, startpos = false, nil, nil, nil

header.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragstart = input.Position
		startpos = main.Position
		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

header.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
		draginput = input
	end
end)

Uis.InputChanged:Connect(function(input)
	if input == draginput and dragging then
		local delta = input.Position - dragstart
		main.Position = UDim2.new(
			startpos.X.Scale, startpos.X.Offset + delta.X,
			startpos.Y.Scale, startpos.Y.Offset + delta.Y
		)
	end
end)
