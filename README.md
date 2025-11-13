--// Stealer Hub (antes "Steal a Rainbow Friends")
--// Feito por Kevin — GUI fiel ao estilo injection-piter (versão menor)
if game.CoreGui:FindFirstChild("StealerHubGUI") then
    game.CoreGui.StealerHubGUI:Destroy()
end

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer

-- ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "StealerHubGUI"
screenGui.ResetOnSpawn = false
screenGui.Parent = game.CoreGui

-- MAIN FRAME (menor)
local main = Instance.new("Frame")
main.Name = "Main"
main.Size = UDim2.new(0, 260, 0, 215)
main.Position = UDim2.new(0.5, -130, 0.45, -90)
main.AnchorPoint = Vector2.new(0.5, 0.5)
main.BackgroundColor3 = Color3.fromRGB(20,20,20)
main.BorderSizePixel = 0
main.Active = true
main.Draggable = true
main.Parent = screenGui
local mainCorner = Instance.new("UICorner", main)
mainCorner.CornerRadius = UDim.new(0,10)

-- Title
local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1, -20, 0, 28)
title.Position = UDim2.new(0, 10, 0, 4)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 18
title.TextColor3 = Color3.fromRGB(160,90,255)
title.TextXAlignment = Enum.TextXAlignment.Left
title.Text = "Stealer Hub"

-- Minimize button
local btnMin = Instance.new("TextButton", main)
btnMin.Size = UDim2.new(0,24,0,24)
btnMin.Position = UDim2.new(1, -32, 0, 4)
btnMin.BackgroundColor3 = Color3.fromRGB(55,55,55)
btnMin.Text = "_"
btnMin.Font = Enum.Font.GothamBold
btnMin.TextSize = 16
btnMin.TextColor3 = Color3.fromRGB(255,255,255)
btnMin.BorderSizePixel = 0
Instance.new("UICorner", btnMin).CornerRadius = UDim.new(1,0)

-- Status
local status = Instance.new("TextLabel", main)
status.Size = UDim2.new(1, -20, 0, 24)
status.Position = UDim2.new(0,10,0,36)
status.BackgroundTransparency = 1
status.Font = Enum.Font.Gotham
status.TextSize = 14
status.TextColor3 = Color3.fromRGB(240,240,240)
status.TextXAlignment = Enum.TextXAlignment.Left
status.Text = "Posição salva: nenhuma"

-- BUTTON CREATOR
local function createButton(parent, text, y)
    local b = Instance.new("TextButton", parent)
    b.Size = UDim2.new(0.86, 0, 0, 34)
    b.Position = UDim2.new(0.07, 0, 0, y)
    b.BackgroundColor3 = Color3.fromRGB(135,95,255)
    b.Text = text
    b.Font = Enum.Font.GothamBold
    b.TextSize = 16
    b.TextColor3 = Color3.fromRGB(255,255,255)
    b.BorderSizePixel = 0
    local cr = Instance.new("UICorner", b)
    cr.CornerRadius = UDim.new(0,8)

    local grad = Instance.new("UIGradient", b)
    grad.Color = ColorSequence.new{
        ColorSequenceKeypoint.new(0, Color3.fromRGB(255,255,255)),
        ColorSequenceKeypoint.new(0.5, Color3.fromRGB(180,150,255)),
        ColorSequenceKeypoint.new(1, Color3.fromRGB(255,255,255))
    }

    local pulse = TweenService:Create(grad, TweenInfo.new(1.2, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true), {Rotation = 360})
    pulse:Play()

    return b
end

local saveBtn = createButton(main, "Salvar posição", 70)
local tpBtn   = createButton(main, "Teleportar", 112)
local speedBtn = createButton(main, "Speed Boost", 154)

-- LOG
local log = Instance.new("TextLabel", main)
log.Size = UDim2.new(1, -20, 0, 20)
log.Position = UDim2.new(0, 10, 0, 188)
log.BackgroundTransparency = 1
log.Font = Enum.Font.Code
log.TextSize = 13
log.TextColor3 = Color3.fromRGB(150,255,150)
log.TextXAlignment = Enum.TextXAlignment.Left
log.Text = "[LOG] Janela restaurada."

-- MINI BUTTON
local mini = Instance.new("TextButton", screenGui)
mini.Name = "MiniSteal"
mini.Size = UDim2.new(0, 60, 0, 34)
mini.Position = UDim2.new(1, -80, 1, -80)
mini.BackgroundColor3 = Color3.fromRGB(10,10,10)
mini.Text = "Steal"
mini.Font = Enum.Font.GothamBold
mini.TextSize = 15
mini.TextColor3 = Color3.fromRGB(255,255,255)
mini.Visible = false
mini.BorderSizePixel = 0
Instance.new("UICorner", mini).CornerRadius = UDim.new(0,8)

-- Vars
local savedPos = nil
local isMin = false

-- Save
saveBtn.MouseButton1Click:Connect(function()
    local char = player.Character or player.CharacterAdded:Wait()
    local root = char:FindFirstChild("HumanoidRootPart")
    if root then
        savedPos = root.Position
        status.Text = "Posição salva!"
        log.Text = "[LOG] Success ✅ Posição salva!"
        log.TextColor3 = Color3.fromRGB(120,255,140)
    end
end)

-- Teleport
tpBtn.MouseButton1Click:Connect(function()
    local char = player.Character or player.CharacterAdded:Wait()
    local root = char and char:FindFirstChild("HumanoidRootPart")
    if savedPos and root then
        root.CFrame = CFrame.new(savedPos + Vector3.new(0,3,0))
        status.Text = "Teleportado!"
        log.Text = "[LOG] Success ✅ Teleportado!"
        log.TextColor3 = Color3.fromRGB(120,255,140)
    else
        log.Text = "[LOG] Nenhuma posição salva!"
        log.TextColor3 = Color3.fromRGB(255,120,120)
    end
end)

-- SPEED BOOST
speedBtn.MouseButton1Click:Connect(function()
    local char = player.Character or player.CharacterAdded:Wait()
    local humanoid = char and char:FindFirstChildOfClass("Humanoid")
    if humanoid then
        humanoid.WalkSpeed = 70
        log.Text = "[LOG] Velocidade aumentada para 70 ⚡"
        log.TextColor3 = Color3.fromRGB(255,220,120)
    else
        log.Text = "[LOG] Humanoid não encontrado!"
        log.TextColor3 = Color3.fromRGB(255,120,120)
    end
end)

-- Minimizar
btnMin.MouseButton1Click:Connect(function()
    if not isMin then
        local t = TweenService:Create(main, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = UDim2.new(0,260,0,0), BackgroundTransparency = 1})
        t:Play()
        t.Completed:Wait()
        main.Visible = false
        mini.Visible = true
        isMin = true
        log.Text = "[LOG] Janela minimizada."
    else
        main.Visible = true
        main.Size = UDim2.new(0,260,0,0)
        TweenService:Create(main, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = UDim2.new(0,260,0,215), BackgroundTransparency = 0}):Play()
        mini.Visible = false
        isMin = false
        log.Text = "[LOG] Janela restaurada."
    end
end)

-- Restaurar via mini
mini.MouseButton1Click:Connect(function()
    mini.Visible = false
    main.Visible = true
    main.Size = UDim2.new(0,260,0,0)
    TweenService:Create(main, TweenInfo.new(0.25, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {Size = UDim2.new(0,260,0,215), BackgroundTransparency = 0}):Play()
    isMin = false
    log.Text = "[LOG] Janela restaurada."
end)

-- Respawn
player.CharacterAdded:Connect(function()
    task.wait(1)
    if isMin then
        main.Visible = false
        mini.Visible = true
    else
        mini.Visible = false
        main.Visible = true
    end
end)
