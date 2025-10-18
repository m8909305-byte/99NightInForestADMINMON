-- ADMINMON All-in-one (Single script)
-- Password: 0612323759MON
-- Put in StarterGui as LocalScript or run in Executor
-----------------------------------------------------

repeat task.wait() until game:IsLoaded()
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UIS = game:GetService("UserInputService")
local Debris = game:GetService("Debris")

local player = Players.LocalPlayer
repeat task.wait() until player and player:FindFirstChild("PlayerGui")
local PlayerGui = player:FindFirstChild("PlayerGui")

-- choose parent (prefer PlayerGui for Studio/normal; CoreGui for some executors)
local guiParent = PlayerGui
pcall(function()
	local cg = game:GetService("CoreGui")
	if cg then guiParent = cg end
end)

-- sound asset id (replace with your own if you want)
local OPEN_SOUND_ID = "rbxassetid://12221967" -- example UI sound; change if desired

-- helper clamp
local function clamp(n,a,b) return math.max(a, math.min(b, n)) end

-- cleanup previous
for _, name in ipairs({"ADMINMON_Auth", "ADMINMON_GUI"}) do
	local prev = guiParent:FindFirstChild(name)
	if prev then prev:Destroy() end
end

-- ---------- Auth modal ----------
local authGui = Instance.new("ScreenGui")
authGui.Name = "ADMINMON_Auth"
authGui.ResetOnSpawn = false
authGui.Parent = guiParent

local modal = Instance.new("Frame", authGui)
modal.Size = UDim2.new(0, 360, 0, 180)
modal.Position = UDim2.new(0.5, -180, 0.5, -90)
modal.BackgroundColor3 = Color3.fromRGB(10,10,10)
modal.BorderSizePixel = 0
modal.Active = true
modal.Draggable = true
Instance.new("UICorner", modal).CornerRadius = UDim.new(0,10)

local title = Instance.new("TextLabel", modal)
title.Size = UDim2.new(1, -20, 0, 36)
title.Position = UDim2.new(0, 10, 0, 10)
title.BackgroundTransparency = 1
title.Text = "⚙️ ADMINMON Access"
title.TextColor3 = Color3.fromRGB(255,215,0)
title.Font = Enum.Font.GothamBold
title.TextSize = 20

local hint = Instance.new("TextLabel", modal)
hint.Size = UDim2.new(1, -20, 0, 20)
hint.Position = UDim2.new(0, 10, 0, 46)
hint.BackgroundTransparency = 1
hint.Text = "Enter admin password to continue"
hint.TextColor3 = Color3.fromRGB(200,200,200)
hint.Font = Enum.Font.Gotham
hint.TextSize = 14

local inputBox = Instance.new("TextBox", modal)
inputBox.Size = UDim2.new(1, -20, 0, 40)
inputBox.Position = UDim2.new(0, 10, 0, 72)
inputBox.PlaceholderText = "Password..."
inputBox.Text = ""
inputBox.ClearTextOnFocus = false
inputBox.BackgroundColor3 = Color3.fromRGB(25,25,25)
inputBox.TextColor3 = Color3.fromRGB(255,255,255)
inputBox.Font = Enum.Font.Gotham
Instance.new("UICorner", inputBox).CornerRadius = UDim.new(0,6)

local submitBtn = Instance.new("TextButton", modal)
submitBtn.Size = UDim2.new(0.46, -6, 0, 36)
submitBtn.Position = UDim2.new(0.02, 0, 1, -44)
submitBtn.Text = "Enter"
submitBtn.BackgroundColor3 = Color3.fromRGB(50,50,50)
submitBtn.TextColor3 = Color3.fromRGB(255,255,255)
Instance.new("UICorner", submitBtn).CornerRadius = UDim.new(0,6)

local cancelBtn = Instance.new("TextButton", modal)
cancelBtn.Size = UDim2.new(0.46, -6, 0, 36)
cancelBtn.Position = UDim2.new(0.52, 0, 1, -44)
cancelBtn.Text = "Cancel"
cancelBtn.BackgroundColor3 = Color3.fromRGB(50,50,50)
cancelBtn.TextColor3 = Color3.fromRGB(255,255,255)
Instance.new("UICorner", cancelBtn).CornerRadius = UDim.new(0,6)

-- small feedback label
local feed = Instance.new("TextLabel", modal)
feed.Size = UDim2.new(1, -20, 0, 18)
feed.Position = UDim2.new(0, 10, 1, -22)
feed.BackgroundTransparency = 1
feed.Text = ""
feed.TextColor3 = Color3.fromRGB(255,100,100)
feed.Font = Enum.Font.Gotham
feed.TextSize = 14

-- function to play open sound and animate GUI
local function playOpenEffect(mainGuiFrame)
	-- sound
	local s = Instance.new("Sound", mainGuiFrame)
	s.SoundId = OPEN_SOUND_ID -- change to a real asset id if needed
	s.Volume = 1
	pcall(function() s:Play() end)
	Debris:AddItem(s, 3)
	-- animation: scale from 0.7 to 1 and fade in
	mainGuiFrame.AnchorPoint = Vector2.new(0.5, 0.5)
	local origin = mainGuiFrame.Position
	mainGuiFrame.Position = UDim2.new(0.5, 0, 0.5, 0)
	mainGuiFrame.Size = UDim2.new(0, math.floor(mainGuiFrame.Size.X.Offset*0.7), 0, math.floor(mainGuiFrame.Size.Y.Offset*0.7))
	mainGuiFrame.BackgroundTransparency = 1
	game:GetService("TweenService"):Create(mainGuiFrame, TweenInfo.new(0.35, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = UDim2.new(0, mainGuiFrame.Size.X.Offset/0.7, 0, mainGuiFrame.Size.Y.Offset/0.7), BackgroundTransparency = 0}):Play()
end

-- ADMIN password
local ADMIN_PASS = "0612323759MON"

-- create main admin GUI (but keep hidden until correct pass)
local adminGui = Instance.new("ScreenGui")
adminGui.Name = "ADMINMON_GUI"
adminGui.ResetOnSpawn = false
adminGui.Parent = guiParent
adminGui.Enabled = false -- hidden until pass

local mainFrame = Instance.new("Frame", adminGui)
mainFrame.Size = UDim2.new(0, 420, 0, 520)
mainFrame.Position = UDim2.new(0.5, -210, 0.5, -260)
mainFrame.BackgroundColor3 = Color3.fromRGB(10,10,10)
mainFrame.Active = true
mainFrame.Draggable = true
Instance.new("UICorner", mainFrame).CornerRadius = UDim.new(0,10)

local topTitle = Instance.new("TextLabel", adminGui)
topTitle.Size = UDim2.new(0,300,0,36)
topTitle.Position = UDim2.new(0.02,0,0.02,0)
topTitle.BackgroundTransparency = 1
topTitle.Text = "⚙️ ADMINMON GUI v1.0"
topTitle.TextColor3 = Color3.fromRGB(255,215,0)
topTitle.Font = Enum.Font.GothamBold
topTitle.TextSize = 20

-- P toggle for admin GUI visibility
local adminVisible = true
UIS.InputBegan:Connect(function(inp, processed)
	if processed then return end
	if inp.KeyCode == Enum.KeyCode.P then
		adminVisible = not adminVisible
		adminGui.Enabled = adminVisible
	end
end)

-- helper: labeled numeric input
local function makeLabeled(y, labelText, default)
	local lbl = Instance.new("TextLabel", mainFrame)
	lbl.Size = UDim2.new(0,180,0,26)
	lbl.Position = UDim2.new(0,12,0,y)
	lbl.BackgroundTransparency = 1
	lbl.Text = labelText
	lbl.TextColor3 = Color3.fromRGB(220,220,220)
	lbl.Font = Enum.Font.Gotham
	lbl.TextSize = 14
	local box = Instance.new("TextBox", mainFrame)
	box.Size = UDim2.new(0,200,0,26)
	box.Position = UDim2.new(0,208,0,y)
	box.BackgroundColor3 = Color3.fromRGB(25,25,25)
	box.TextColor3 = Color3.fromRGB(255,255,255)
	box.Text = tostring(default)
	box.Font = Enum.Font.Gotham
	box.TextSize = 14
	Instance.new("UICorner", box).CornerRadius = UDim.new(0,6)
	return box
end

-- create controls
local walkBox = makeLabeled(60, "WalkSpeed (1-1000):", 50)
local flyBox = makeLabeled(110, "FlySpeed (1-500):", 150)
local jumpBox = makeLabeled(160, "JumpPower (1-100):", 70)
local dmgBox = makeLabeled(210, "Melee Damage (1-100):", 20)

-- vacuum names display
local vacLabel = Instance.new("TextLabel", mainFrame)
vacLabel.Size = UDim2.new(0,180,0,26)
vacLabel.Position = UDim2.new(0,12,0,260)
vacLabel.BackgroundTransparency = 1
vacLabel.Text = "Vacuum targets (names):"
vacLabel.TextColor3 = Color3.fromRGB(220,220,220)
vacLabel.Font = Enum.Font.Gotham
vacLabel.TextSize = 14
local vacListLabel = Instance.new("TextLabel", mainFrame)
vacListLabel.Size = UDim2.new(0,200,0,40)
vacListLabel.Position = UDim2.new(0,208,0,260)
vacListLabel.BackgroundTransparency = 1
vacListLabel.Text = "Coal, OilCan, BigFuelTank"
vacListLabel.TextColor3 = Color3.fromRGB(200,200,200)
vacListLabel.Font = Enum.Font.Gotham
vacListLabel.TextSize = 14
vacListLabel.TextWrapped = true

-- button maker
local function makeBtn(text, y, color)
	local b = Instance.new("TextButton", mainFrame)
	b.Size = UDim2.new(0,384,0,34)
	b.Position = UDim2.new(0,18,0,y)
	b.Text = text
	b.Font = Enum.Font.GothamBold
	b.TextSize = 16
	b.BackgroundColor3 = color or Color3.fromRGB(45,45,45)
	b.TextColor3 = Color3.fromRGB(255,255,255)
	Instance.new("UICorner", b).CornerRadius = UDim.new(0,6)
	return b
end

-- store runtime state
local humanoid, root = nil, nil
local function refreshChar()
	local char = player.Character or player.CharacterAdded:Wait()
	humanoid = char:WaitForChild("Humanoid")
	root = char:WaitForChild("HumanoidRootPart")
end
refreshChar()
player.CharacterAdded:Connect(refreshChar)

-- Apply walk speed
makeBtn("Apply WalkSpeed", 320, Color3.fromRGB(80,160,255)).MouseButton1Click:Connect(function()
	local n = tonumber(walkBox.Text) or 50
	n = clamp(n,1,1000)
	if humanoid then humanoid.WalkSpeed = n end
end)

-- Fly toggle
local isFlying = false
local flyBV, flyConn
local function startFly(speed)
	if not root then return end
	if flyBV then flyBV:Destroy() end
	flyBV = Instance.new("BodyVelocity")
	flyBV.MaxForce = Vector3.new(1e5,1e5,1e5)
	flyBV.Velocity = Vector3.new(0,0,0)
	flyBV.Parent = root
	isFlying = true
	flyConn = RunService.Heartbeat:Connect(function()
		if not isFlying then return end
		local cam = workspace.CurrentCamera
		local dir = Vector3.new()
		if UIS:IsKeyDown(Enum.KeyCode.W) then dir += cam.CFrame.LookVector end
		if UIS:IsKeyDown(Enum.KeyCode.S) then dir -= cam.CFrame.LookVector end
		if UIS:IsKeyDown(Enum.KeyCode.A) then dir -= cam.CFrame.RightVector end
		if UIS:IsKeyDown(Enum.KeyCode.D) then dir += cam.CFrame.RightVector end
		if UIS:IsKeyDown(Enum.KeyCode.Space) then dir += Vector3.new(0,1,0) end
		if UIS:IsKeyDown(Enum.KeyCode.LeftControl) then dir -= Vector3.new(0,1,0) end
		local v = (dir.Magnitude > 0 and dir.Unit * speed) or Vector3.new()
		if flyBV then flyBV.Velocity = v end
	end)
end
local function stopFly()
	isFlying = false
	if flyConn then flyConn:Disconnect(); flyConn=nil end
	if flyBV then flyBV:Destroy(); flyBV=nil end
end
makeBtn("Toggle Fly (use WASD + Space/LCtrl)", 360, Color3.fromRGB(120,200,255)).MouseButton1Click:Connect(function()
	if not isFlying then
		local sp = clamp(tonumber(flyBox.Text) or 150, 1, 500)
		startFly(sp)
	else
		stopFly()
	end
end)

-- Apply JumpPower (and small impulse)
makeBtn("Apply JumpPower (and small boost)", 400, Color3.fromRGB(120,240,120)).MouseButton1Click:Connect(function()
	local j = clamp(tonumber(jumpBox.Text) or 70, 1, 100)
	if humanoid then humanoid.JumpPower = j end
	-- immediate upward impulse for feel
	if root then
		local ibv = Instance.new("BodyVelocity")
		ibv.MaxForce = Vector3.new(0,1e5,0)
		ibv.Velocity = Vector3.new(0, math.min(220, j*3), 0)
		ibv.Parent = root
		Debris:AddItem(ibv, 0.25)
	end
end)

-- Apply melee damage (attach to tools)
makeBtn("Apply Melee Damage (attach to tool handles)", 440, Color3.fromRGB(255,140,80)).MouseButton1Click:Connect(function()
	local dmg = clamp(tonumber(dmgBox.Text) or 20, 1, 100)
	local function attach(tool)
		if tool and tool:FindFirstChild("Handle") then
			local h = tool.Handle
			h.Touched:Connect(function(hit)
				local tHum = hit.Parent and hit.Parent:FindFirstChildOfClass("Humanoid")
				if tHum and tHum ~= humanoid then
					if tHum.Health and tHum.Health > 0 then
						-- locally try to reduce health (server should handle in real game)
						pcall(function() tHum:TakeDamage(dmg) end)
					end
				end
			end)
		end
	end
	for _, t in pairs(player.Character:GetChildren()) do if t:IsA("Tool") then attach(t) end end
	player.Character.ChildAdded:Connect(function(c) if c:IsA("Tool") then attach(c) end end)
end)

-- Vacuum (Coal, OilCan, BigFuelTank)
local vacuumOn = false
local vacuumConn
local targetNames = { "Coal", "OilCan", "BigFuelTank" }
local function isTarget(obj)
	for _,n in ipairs(targetNames) do if obj.Name == n then return true end end
	return false
end

local function startVacuum()
	if vacuumOn then return end
	vacuumOn = true
	vacuumConn = RunService.Heartbeat:Connect(function()
		if not root then return end
		for _, obj in pairs(workspace:GetDescendants()) do
			if obj:IsA("BasePart") and isTarget(obj) then
				local dist = (obj.Position - root.Position).Magnitude
				if dist < 80 then
					local vb = obj:FindFirstChild("VacuumBV")
					if not vb then
						vb = Instance.new("BodyVelocity")
						vb.Name = "VacuumBV"
						vb.MaxForce = Vector3.new(1e5,1e5,1e5)
						vb.Parent = obj
					end
					if obj.Position ~= root.Position then
						local dir = (root.Position - obj.Position)
						vb.Velocity = dir.Unit * 80
					end
					if (obj.Position - root.Position).Magnitude < 3 then
						if vb then vb:Destroy() end
						-- attach to player slightly below root
						pcall(function() obj.CFrame = root.CFrame * CFrame.new(0, -2, 0) end)
					end
				end
			end
		end
	end)
end
local function stopVacuum()
	vacuumOn = false
	if vacuumConn then vacuumConn:Disconnect(); vacuumConn=nil end
	for _, o in pairs(workspace:GetDescendants()) do
		if o:IsA("BasePart") then
			local vb = o:FindFirstChild("VacuumBV")
			if vb then vb:Destroy() end
		end
	end
end
local vacBtn = makeBtn("Toggle Vacuum (Coal/OilCan/BigFuelTank)", 480, Color3.fromRGB(255,160,60))
vacBtn.MouseButton1Click:Connect(function()
	if vacuumOn then stopVacuum() vacBtn.Text = "Toggle Vacuum (Coal/OilCan/BigFuelTank)" else startVacuum() vacBtn.Text = "Vacuum: ON" end
end)

-- Noclip toggle (not floor)
local noclipOn = false
local noclipConn
local function startNoclip()
	noclipOn = true
	noclipConn = RunService.Stepped:Connect(function()
		if player.Character then
			for _, p in pairs(player.Character:GetDescendants()) do
				if p:IsA("BasePart") and p.Name ~= "HumanoidRootPart" then
					p.CanCollide = false
				end
			end
		end
	end)
end
local function stopNoclip()
	noclipOn = false
	if noclipConn then noclipConn:Disconnect(); noclipConn=nil end
end
makeBtn("Toggle NoClip (not floor)", 520, Color3.fromRGB(200,120,200)).MouseButton1Click:Connect(function()
	if noclipOn then stopNoclip() else startNoclip() end
end)

-- Teleport: single click to teleport
makeBtn("Teleport: Click to teleport once", 560, Color3.fromRGB(255,200,120)).MouseButton1Click:Connect(function()
	local m = player:GetMouse()
	local conn
	conn = m.Button1Down:Connect(function()
		local pos = m.Hit and m.Hit.p
		if pos then
			pcall(function() player.Character:MoveTo(pos) end)
		end
		if conn then conn:Disconnect() end
	end)
end)

-- Infinite jump toggle (persistent until rejoin)
makeBtn("Toggle Infinite Jump", 600, Color3.fromRGB(180,120,255)).MouseButton1Click:Connect(function()
	local conn
	conn = UIS.JumpRequest:Connect(function()
		if humanoid and humanoid.Health > 0 then
			humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
		end
	end)
	-- cannot easily auto-disconnect; user can rejoin to reset if necessary
end)

-- Apply melee power instant (example: also set JumpPower high to simulate strong hit)
makeBtn("Apply Strong Hit Instant (temp)", 640, Color3.fromRGB(255,120,80)).MouseButton1Click:Connect(function()
	local p = clamp(tonumber(dmgBox.Text) or 20,1,100)
	-- give player a temporary jump power to simulate stronger hit capability
	if humanoid then
		humanoid.JumpPower = math.min(250, (humanoid.JumpPower or 50) + 50)
	end
end)

-- close button
makeBtn("Close ADMINMON GUI", 680, Color3.fromRGB(120,120,120)).MouseButton1Click:Connect(function()
	adminGui.Enabled = false
end)

-- When submit pressed in auth modal
submitBtn.MouseButton1Click:Connect(function()
	if tostring(inputBox.Text or "") == ADMIN_PASS then
		-- success: destroy auth UI, enable admin GUI, play animation/sound
		authGui:Destroy()
		adminGui.Enabled = true
		task.spawn(function() playOpenEffect(mainFrame) end)
	else
		-- wrong: feedback shake
		feed.Text = "Incorrect password!"
		for i=1,4 do
			modal.Position = modal.Position + UDim2.new(0.01,0,0,0)
			task.wait(0.03)
			modal.Position = modal.Position - UDim2.new(0.01,0,0,0)
			task.wait(0.03)
		end
		inputBox.Text = ""
		task.delay(2, function() feed.Text = "" end)
	end
end)

cancelBtn.MouseButton1Click:Connect(function()
	-- close whole auth GUI
	authGui:Destroy()
end)

-- Final ready print
print("ADMINMON auth ready. Enter password to open admin GUI.")
