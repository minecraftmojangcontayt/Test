--// Stealer Hub (antes "Steal a Rainbow Friends")
--// Feito por Kevin — GUI fiel ao estilo injection-piter (versão menor e sem delay bug)

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
screenGui.IgnoreGuiInset = true
screenGui.Parent = game.CoreGui

-- MAIN FRAME
local main = Instance.new("Frame")
main.Name = "Main"
main.Size = UDim2.new(0, 260, 0, 180)
main.Position = UDim2.new(0.5, -130, 0.45, -90)
main.AnchorPoint = Vector2.new(0.5, 0.5)
main.BackgroundColor3 = Color3.fromRGB(20,20,20)
main.BorderSizePixel = 0
main.Active = true
main.Draggable = true
main.Parent = screenGui
Instance.new("UICorner", main).CornerRadius = UDim.new(0,10)

-- Título
local title = Instance.new("TextLabel", main)
title.Size = UDim2.new(1, -20, 0, 28)
title.Position = UDim2.new(0, 10, 0, 4)
title.BackgroundTransparency = 1
title.Font = Enum.Font.GothamBold
title.TextSize = 18
title.TextColor3 = Color3.fromRGB(160,90,255)
title.TextXAlignment = Enum.TextXAlignment.Left
title.Text = "Stealer Hub"

-- Botão minimizar
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

-- Criação de botões
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
    Instance.new("UICorner", b).CornerRadius = UDim.new(0,8)

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

-- Log
local log = Instance.new("TextLabel", main)
log.Size = UDim2.new(1, -20, 0, 20)
log.Position = UDim2.new(0, 10, 0, 152)
log.BackgroundTransparency = 1
log.Font = Enum.Font.Code
log.TextSize = 13
log.TextColor3 = Color3.fromRGB(150,255,150)
log.TextXAlignment = Enum.TextXAlignment.Left
log.Text = "[LOG] Janela carregada."

-- Mini botão
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

-- Variáveis
local savedPos = nil
local isMin = false

-- Salvar posição
saveBtn.MouseButton1Click:Connect(function()
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") then
        savedPos = char.HumanoidRootPart.Position
        status.Text = "Posição salva!"
        log.Text = "[LOG] ✅ Posição salva!"
        log.TextColor3 = Color3.fromRGB(120,255,140)
    else
        log.Text = "[LOG] ❌ Jogador não encontrado!"
        log.TextColor3 = Color3.fromRGB(255,120,120)
    end
end)

-- Teleportar
tpBtn.MouseButton1Click:Connect(function()
    local char = player.Character
    if char and savedPos and char:FindFirstChild("HumanoidRootPart") then
        char:MoveTo(savedPos + Vector3.new(0,3,0))
        status.Text = "Teleportado!"
        log.Text = "[LOG] ✅ Teleportado!"
        log.TextColor3 = Color3.fromRGB(120,255,140)
    else
        log.Text = "[LOG] ⚠️ Nenhuma posição salva!"
        log.TextColor3 = Color3.fromRGB(255,120,120)
    end
end)

-- Minimizar / Restaurar
btnMin.MouseButton1Click:Connect(function()
    isMin = not isMin
    if isMin then
        TweenService:Create(main, TweenInfo.new(0.25), {Size = UDim2.new(0,260,0,0), BackgroundTransparency = 1}):Play()
        task.delay(0.25, function()
            main.Visible = false
            mini.Visible = true
        end)
        log.Text = "[LOG] Janela minimizada."
    else
        main.Visible = true
        mini.Visible = false
        main.Size = UDim2.new(0,260,0,0)
        TweenService:Create(main, TweenInfo.new(0.25), {Size = UDim2.new(0,260,0,180), BackgroundTransparency = 0}):Play()
        log.Text = "[LOG] Janela restaurada."
    end
end)

-- Restaurar via mini botão
mini.MouseButton1Click:Connect(function()
    isMin = false
    mini.Visible = false
    main.Visible = true
    main.Size = UDim2.new(0,260,0,0)
    TweenService:Create(main, TweenInfo.new(0.25), {Size = UDim2.new(0,260,0,180), BackgroundTransparency = 0}):Play()
    log.Text = "[LOG] Janela restaurada."
end)

-- Recarregar GUI no respawn sem delay nem sumiço
player.CharacterAdded:Connect(function()
    task.defer(function()
        if isMin then
            main.Visible = false
            mini.Visible = true
        else
            mini.Visible = false
            main.Visible = true
        end
    end)
end)
