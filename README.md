-- KEVIN HUB TESTER - COMPLETO
local Players = game:GetService("Players")
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local Camera = workspace.CurrentCamera

local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

-- GUI
local gui = Instance.new("ScreenGui")
gui.Name = "KevinHub"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

-- BOTÃO ABRIR
local openBtn = Instance.new("TextButton", gui)
openBtn.Size = UDim2.new(0, 40, 0, 40)
openBtn.Position = UDim2.new(0, 10, 0.5, -20)
openBtn.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
openBtn.Text = "K"
openBtn.TextColor3 = Color3.new(1, 1, 1)
openBtn.Font = Enum.Font.GothamBlack
openBtn.TextSize = 20
openBtn.Visible = false
Instance.new("UICorner", openBtn).CornerRadius = UDim.new(1, 0)

-- FRAME PRINCIPAL
local mainFrame = Instance.new("Frame", gui)
mainFrame.Size = UDim2.new(0, 450, 0, 240)
mainFrame.Position = UDim2.new(0.5, -225, 0, 20)
mainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
mainFrame.Active = true
mainFrame.Draggable = true
mainFrame.ClipsDescendants = true
Instance.new("UICorner", mainFrame).CornerRadius = UDim.new(0, 15)

-- MENU LATERAL
local left = Instance.new("Frame", mainFrame)
left.Size = UDim2.new(0, 80, 1, 0)
left.Position = UDim2.new(0, 0, 0, 0)
left.BackgroundColor3 = Color3.fromRGB(15, 15, 15)
Instance.new("UICorner", left).CornerRadius = UDim.new(0, 0)

local sectionLabel = Instance.new("TextLabel", left)
sectionLabel.Text = "Kevin"
sectionLabel.Size = UDim2.new(1, 0, 0, 30)
sectionLabel.Position = UDim2.new(0, 0, 0, 10)
sectionLabel.TextColor3 = Color3.new(1, 1, 1)
sectionLabel.BackgroundTransparency = 1
sectionLabel.Font = Enum.Font.GothamBold
sectionLabel.TextSize = 16

-- BOTÃO FECHAR
local closeBtn = Instance.new("TextButton", mainFrame)
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -35, 0, 5)
closeBtn.Text = "❌"
closeBtn.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
closeBtn.TextColor3 = Color3.new(1, 1, 1)
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 14
Instance.new("UICorner", closeBtn).CornerRadius = UDim.new(1, 0)

-- SCROLLING BUTTONS
local buttonFrame = Instance.new("ScrollingFrame", mainFrame)
buttonFrame.Size = UDim2.new(1, -100, 0, 50)
buttonFrame.Position = UDim2.new(0, 90, 0, 40)
buttonFrame.BackgroundTransparency = 1
buttonFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
buttonFrame.ScrollBarThickness = 6

local layout = Instance.new("UIListLayout", buttonFrame)
layout.FillDirection = Enum.FillDirection.Horizontal
layout.SortOrder = Enum.SortOrder.LayoutOrder
layout.Padding = UDim.new(0, 8)

-- NOTIFICAÇÃO
local function createNotification(text)
	local notif = Instance.new("TextLabel", gui)
	notif.Size = UDim2.new(0, 250, 0, 40)
	notif.Position = UDim2.new(0.5, 0, 0, 5)
	notif.AnchorPoint = Vector2.new(0.5, 0)
	notif.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	notif.TextColor3 = Color3.new(1, 1, 1)
	notif.Text = text
	notif.Font = Enum.Font.GothamMedium
	notif.TextSize = 16
	notif.BackgroundTransparency = 1
	notif.TextTransparency = 1
	notif.BorderSizePixel = 0
	Instance.new("UICorner", notif).CornerRadius = UDim.new(0, 8)

	TweenService:Create(notif, TweenInfo.new(0.3), {
		BackgroundTransparency = 0,
		TextTransparency = 0
	}):Play()

	task.delay(1, function()
		TweenService:Create(notif, TweenInfo.new(0.3), {
			BackgroundTransparency = 1,
			TextTransparency = 1
		}):Play()
		wait(0.3)
		notif:Destroy()
	end)
end

-- ESTADOS
local states = {
	Speed = false,
	Aimbot = false,
	ESP = false,
	Jump = false,
	Noclip = false,
	InfiniteJump = false,
	Fling = false,
}

-- FUNÇÕES COMPORTAMENTO
local speedConn, aimbotConn, noclipConn, infJumpConn
local espFolder = Instance.new("Folder", gui)

function setSpeed(on)
	if speedConn then speedConn:Disconnect() end
	humanoid.WalkSpeed = on and 50 or 16
	if on then
		speedConn = RunService.RenderStepped:Connect(function()
			humanoid.WalkSpeed = 50
		end)
	end
end

function setJump(on)
	humanoid.UseJumpPower = true
	humanoid.JumpPower = on and 100 or 50
end

function setInfiniteJump(on)
	if infJumpConn then infJumpConn:Disconnect() end
	if on then
		infJumpConn = UIS.JumpRequest:Connect(function()
			humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
		end)
	end
end

function setFling(on)
	local root = character:FindFirstChild("HumanoidRootPart")
	if not root then return end
	if on then
		local fling = Instance.new("BodyAngularVelocity", root)
		fling.Name = "Flinger"
		fling.AngularVelocity = Vector3.new(999999, 999999, 999999)
		fling.MaxTorque = Vector3.new(999999, 999999, 999999)
	else
		local f = root:FindFirstChild("Flinger")
		if f then f:Destroy() end
	end
end

function setAimbot(on)
	if aimbotConn then RunService:UnbindFromRenderStep("Aimbot") end
	if on then
		RunService:BindToRenderStep("Aimbot", Enum.RenderPriority.Camera.Value + 1, function()
			local closest, shortest = nil, math.huge
			for _, plr in ipairs(Players:GetPlayers()) do
				if plr ~= player and plr.Character and plr.Character:FindFirstChild("Head") then
					local head = plr.Character.Head
					local pos, vis = Camera:WorldToViewportPoint(head.Position)
					if vis then
						local dist = (Vector2.new(pos.X, pos.Y) - UIS:GetMouseLocation()).Magnitude
						if dist < shortest then
							shortest = dist
							closest = head
						end
					end
				end
			end
			if closest then Camera.CFrame = CFrame.new(Camera.CFrame.Position, closest.Position) end
		end)
	end
end

function setESP(on)
	espFolder:ClearAllChildren()
	if on then
		for _, plr in ipairs(Players:GetPlayers()) do
			if plr ~= player and plr.Character then
				local h = Instance.new("Highlight", espFolder)
				h.Adornee = plr.Character
				h.FillColor = Color3.fromRGB(255, 0, 0)
				h.OutlineColor = Color3.new(1, 1, 1)
			end
		end
	end
end

function setNoclip(on)
	if noclipConn then noclipConn:Disconnect() end
	if on then
		noclipConn = RunService.Stepped:Connect(function()
			for _, part in ipairs(character:GetDescendants()) do
				if part:IsA("BasePart") then
					part.CanCollide = false
				end
			end
		end)
	end
end

-- BOTÕES
local features = {
	{Name = "Speed", Func = setSpeed},
	{Name = "Aimbot", Func = setAimbot},
	{Name = "ESP", Func = setESP},
	{Name = "Jump", Func = setJump},
	{Name = "Noclip", Func = setNoclip},
	{Name = "InfiniteJump", Func = setInfiniteJump},
	{Name = "Fling", Func = setFling},
}

for _, feature in ipairs(features) do
	local btn = Instance.new("TextButton", buttonFrame)
	btn.Size = UDim2.new(0, 110, 1, -10)
	btn.Text = feature.Name .. " [OFF]"
	btn.TextColor3 = Color3.new(1, 1, 1)
	btn.Font = Enum.Font.GothamBold
	btn.TextScaled = true
	btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	btn.BorderSizePixel = 0
	Instance.new("UICorner", btn).CornerRadius = UDim.new(1, 0)

	btn.MouseButton1Click:Connect(function()
		local state = not states[feature.Name]
		states[feature.Name] = state
		btn.Text = feature.Name .. (state and " [ON]" or " [OFF]")
		btn.BackgroundColor3 = state and Color3.fromRGB(0, 170, 0) or Color3.fromRGB(50, 50, 50)
		createNotification("Success: " .. feature.Name .. (state and " ativado" or " desativado"))
		feature.Func(state)
	end)
end

layout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(function()
	buttonFrame.CanvasSize = UDim2.new(0, layout.AbsoluteContentSize.X + 10, 0, 0)
end)

-- HAT HUB
local hatHubBtn = Instance.new("TextButton", mainFrame)
hatHubBtn.Size = UDim2.new(0, 100, 0, 30)
hatHubBtn.Position = UDim2.new(0, 100, 0, 100)
hatHubBtn.Text = "Hat Hub"
hatHubBtn.Font = Enum.Font.GothamBold
hatHubBtn.TextColor3 = Color3.new(1, 1, 1)
hatHubBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
Instance.new("UICorner", hatHubBtn).CornerRadius = UDim.new(0, 8)

hatHubBtn.MouseButton1Click:Connect(function()
	loadstring(game:HttpGet("https://rawscripts.net/raw/Just-a-baseplate.-hathub-new-36363"))()
	createNotification("Hat Hub loaded ✅")
end)

-- HAT ID SYSTEM
local hatBtn = Instance.new("TextButton", mainFrame)
hatBtn.Size = UDim2.new(0, 100, 0, 30)
hatBtn.Position = UDim2.new(0, 210, 0, 100)
hatBtn.Text = "Hat"
hatBtn.Font = Enum.Font.GothamBold
hatBtn.TextColor3 = Color3.new(1, 1, 1)
hatBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
Instance.new("UICorner", hatBtn).CornerRadius = UDim.new(0, 8)

local hatBox = Instance.new("TextBox", mainFrame)
hatBox.Size = UDim2.new(0, 140, 0, 30)
hatBox.Position = UDim2.new(0, 100, 0, 140)
hatBox.PlaceholderText = "Enter Hat ID"
hatBox.Visible = false
hatBox.Font = Enum.Font.Gotham
hatBox.TextColor3 = Color3.new(1, 1, 1)
hatBox.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
Instance.new("UICorner", hatBox).CornerRadius = UDim.new(0, 8)

local hatConfirm = Instance.new("TextButton", mainFrame)
hatConfirm.Size = UDim2.new(0, 60, 0, 30)
hatConfirm.Position = UDim2.new(0, 250, 0, 140)
hatConfirm.Text = "Equip"
hatConfirm.Visible = false
hatConfirm.Font = Enum.Font.GothamBold
hatConfirm.TextColor3 = Color3.new(1, 1, 1)
hatConfirm.BackgroundColor3 = Color3.fromRGB(40, 100, 40)
Instance.new("UICorner", hatConfirm).CornerRadius = UDim.new(0, 8)

hatBtn.MouseButton1Click:Connect(function()
	hatBox.Visible = not hatBox.Visible
	hatConfirm.Visible = hatBox.Visible
end)

hatConfirm.MouseButton1Click:Connect(function()
	local id = tonumber(hatBox.Text)
	if not id then
		createNotification("ID inválido ❌")
		return
	end
	local ok, obj = pcall(function() return game:GetObjects("rbxassetid://"..id)[1] end)
	if ok and obj and obj:IsA("Accessory") then
		obj.Parent = character
		humanoid:AddAccessory(obj)
		createNotification("Hat equipado ✅")
	else
		createNotification("ID inválido ❌")
	end
end)

-- OPEN / CLOSE
closeBtn.MouseButton1Click:Connect(function()
	mainFrame.Visible = false
	openBtn.Visible = true
end)

openBtn.MouseButton1Click:Connect(function()
	mainFrame.Visible = true
	openBtn.Visible = false
end)

-- RELOAD POWERS AFTER DEATH
player.CharacterAdded:Connect(function(char)
	character = char
	humanoid = char:WaitForChild("Humanoid")
	for name, state in pairs(states) do
		if state and _G["set"..name] then
			_G["set"..name](true)
		end
	end
end)
