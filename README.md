local Players = game:GetService("Players")
local TextChatService = game:GetService("TextChatService")
local Lighting = game:GetService("Lighting")
local MaterialService = game:GetService("MaterialService")
local SoundService = game:GetService("SoundService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Workspace = game:GetService("Workspace")
 
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local camera = Workspace.CurrentCamera
 
-- GUI principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ConfiguracaoGUI"
screenGui.ResetOnSpawn = false
screenGui.IgnoreGuiInset = true
screenGui.Parent = playerGui
 
-- Escala DPI
local uiScale = Instance.new("UIScale")
uiScale.Parent = screenGui
 
local function ajustarUIScale()
    local size = camera.ViewportSize
    local menor = math.min(size.X, size.Y)
    uiScale.Scale = menor / 680
end
ajustarUIScale()
camera:GetPropertyChangedSignal("ViewportSize"):Connect(ajustarUIScale)
 
-- Blur
local blur = Instance.new("BlurEffect")
blur.Size = 12
blur.Enabled = false
blur.Parent = Lighting
 
-- Botão engrenagem
local gearButton = Instance.new("ImageButton")
gearButton.Size = UDim2.new(0, 66, 0, 53)
gearButton.Position = UDim2.new(0, 10, 0, 330)
gearButton.Image = "rbxassetid://14306059173"
gearButton.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
gearButton.BorderColor3 = Color3.fromRGB(255, 255, 255)
gearButton.BorderSizePixel = 3
gearButton.Parent = screenGui
Instance.new("UICorner", gearButton).CornerRadius = UDim.new(0, 8)
 
-- Frame principal
local configFrame = Instance.new("Frame")
configFrame.Size = UDim2.new(0, 500, 0, 445)
configFrame.Position = UDim2.new(0.5, -249, 0.5, -215)
configFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
configFrame.BackgroundTransparency = 0 -- preto sólido
configFrame.BorderColor3 = Color3.fromRGB(255, 255, 255)
configFrame.BorderSizePixel = 4
configFrame.Visible = false
configFrame.Parent = screenGui
Instance.new("UICorner", configFrame).CornerRadius = UDim.new(0, 10)
 
-- Botão fechar
local closeBtn = Instance.new("TextButton")
closeBtn.Text = "X"
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -35, 0, 5)
closeBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
closeBtn.TextColor3 = Color3.new(1, 1, 1)
closeBtn.Font = Enum.Font.SourceSansBold
closeBtn.TextScaled = true
closeBtn.Parent = configFrame
Instance.new("UICorner", closeBtn).CornerRadius = UDim.new(0, 6)
 
-- Título
local titulo = Instance.new("TextLabel")
titulo.Text = "⚙️ Configurações ⚙️"
titulo.Size = UDim2.new(1, 0, 0, 40)
titulo.BackgroundTransparency = 1
titulo.TextColor3 = Color3.new(1, 1, 1)
titulo.Font = Enum.Font.SourceSansBold
titulo.TextScaled = true
titulo.Parent = configFrame
 
-- Scroll
local scroll = Instance.new("ScrollingFrame")
scroll.Size = UDim2.new(1, -20, 1, -60)
scroll.Position = UDim2.new(0, 10, 0, 50)
scroll.CanvasSize = UDim2.new(0, 0, 8, 0)
scroll.ScrollBarThickness = 6
scroll.BackgroundTransparency = 1
scroll.Parent = configFrame
 
-- Switch (On/Off) com bolinha animada
local function criarBotaoOnOff(nome, descricao, posY, callback)
    local label = Instance.new("TextLabel", scroll)
    label.Text = nome
    label.Position = UDim2.new(0, 10, 0, posY)
    label.Size = UDim2.new(0, 200, 0, 25)
    label.TextColor3 = Color3.new(1, 1, 1)
    label.BackgroundTransparency = 1
    label.Font = Enum.Font.SourceSansBold
    label.TextScaled = true
    label.TextXAlignment = Enum.TextXAlignment.Left
    
    local switchFrame = Instance.new("Frame", scroll)
    switchFrame.Size = UDim2.new(0, 60, 0, 30)
    switchFrame.Position = UDim2.new(0, 220, 0, posY)
    switchFrame.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    switchFrame.BorderColor3 = Color3.new(1, 1, 1)
    switchFrame.BorderSizePixel = 1
    Instance.new("UICorner", switchFrame).CornerRadius = UDim.new(1, 0)
    
    local ball = Instance.new("Frame", switchFrame)
    ball.Size = UDim2.new(0, 24, 0, 24)
    ball.Position = UDim2.new(0, 3, 0, 3)
    ball.BackgroundColor3 = Color3.new(1, 1, 1)
    ball.BorderColor3 = Color3.new(0, 0, 0)
    ball.BorderSizePixel = 1
    Instance.new("UICorner", ball).CornerRadius = UDim.new(1, 0)
    
    local button = Instance.new("TextButton", switchFrame)
    button.Size = UDim2.new(1, 0, 1, 0)
    button.BackgroundTransparency = 1
    button.Text = ""
    
    local ligado = false
    local function atualizarVisual(animar)
        if ligado then
            switchFrame.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
            TweenService:Create(ball, TweenInfo.new(animar and 0.2 or 0), {Position = UDim2.new(1, -27, 0, 3)}):Play()
        else
            switchFrame.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
            TweenService:Create(ball, TweenInfo.new(animar and 0.2 or 0), {Position = UDim2.new(0, 3, 0, 3)}):Play()
        end
    end
    
    button.MouseButton1Click:Connect(function()
        ligado = not ligado
        atualizarVisual(true)
        callback(ligado)
    end)
    
    atualizarVisual(false)
    
    local desc = Instance.new("TextLabel", scroll)
    desc.Text = descricao
    desc.Size = UDim2.new(0.95, 0, 0, 30)
    desc.Position = UDim2.new(0.025, 0, 0, posY + 30)
    desc.TextColor3 = Color3.fromRGB(200, 200, 200)
    desc.BackgroundTransparency = 1
    desc.Font = Enum.Font.SourceSans
    desc.TextSize = 14
    desc.TextWrapped = true
    desc.TextXAlignment = Enum.TextXAlignment.Left
end
 
-- Slider
local function criarSlider(nome, valorInicial, descricao, posY, aplicarVolume)
    local label = Instance.new("TextLabel", scroll)
    label.Text = nome
    label.Position = UDim2.new(0, 10, 0, posY)
    label.Size = UDim2.new(0, 200, 0, 25)
    label.TextColor3 = Color3.new(1, 1, 1)
    label.BackgroundTransparency = 1
    label.Font = Enum.Font.SourceSansBold
    label.TextScaled = true
    label.TextXAlignment = Enum.TextXAlignment.Left
    
    local porcento = Instance.new("TextLabel", scroll)
    porcento.Text = valorInicial .. "%"
    porcento.Position = UDim2.new(0, 220, 0, posY)
    porcento.Size = UDim2.new(0, 50, 0, 25)
    porcento.BackgroundTransparency = 1
    porcento.TextColor3 = Color3.new(1, 1, 1)
    porcento.Font = Enum.Font.SourceSansBold
    porcento.TextScaled = true
    
    local barra = Instance.new("Frame", scroll)
    barra.Size = UDim2.new(0, 150, 0, 5)
    barra.Position = UDim2.new(0, 290, 0, posY + 10)
    barra.BackgroundColor3 = Color3.fromRGB(180, 180, 180)
    barra.BorderSizePixel = 0
    
    local indicador = Instance.new("Frame", barra)
    indicador.Size = UDim2.new(0, 10, 0, 20)
    indicador.Position = UDim2.new(0, (valorInicial / 100) * 150 - 5, 0, -7)
    indicador.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    indicador.BorderSizePixel = 0
    
    local dragging = false
    indicador.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
        end
    end)
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local x = math.clamp(input.Position.X - barra.AbsolutePosition.X, 0, 150)
            indicador.Position = UDim2.new(0, x - 5, 0, -7)
            local porcentagem = math.floor((x / 150) * 100)
            porcento.Text = porcentagem .. "%"
            aplicarVolume(porcentagem)
        end
    end)
    
    local desc = Instance.new("TextLabel", scroll)
    desc.Text = descricao
    desc.Size = UDim2.new(0.95, 0, 0, 30)
    desc.Position = UDim2.new(0.025, 0, 0, posY + 30)
    desc.TextColor3 = Color3.fromRGB(200, 200, 200)
    desc.BackgroundTransparency = 1
    desc.Font = Enum.Font.SourceSans
    desc.TextSize = 14
    desc.TextWrapped = true
    desc.TextXAlignment = Enum.TextXAlignment.Left
end
 
-- Configurações
criarBotaoOnOff("Textura", "Reduz a textura do jogo para melhorar desempenho.", 40, function(e)
    MaterialService.Use2022Materials = e
end)
criarBotaoOnOff("Sombra", "Ativa ou desativa as sombras globais.", 130, function(e)
    Lighting.GlobalShadows = e
end)
 
local audioTitulo = Instance.new("TextLabel", scroll)
audioTitulo.Text = "Áudio"
audioTitulo.Size = UDim2.new(1, 0, 0, 30)
audioTitulo.Position = UDim2.new(0, 0, 0, 200)
audioTitulo.BackgroundTransparency = 1
audioTitulo.TextColor3 = Color3.new(1, 1, 1)
audioTitulo.Font = Enum.Font.SourceSansBold
audioTitulo.TextScaled = true
audioTitulo.Parent = scroll
 
criarSlider("Geral", 50, "Volume total do jogo.", 240, function(v)
    SoundService.Volume = v / 100
end)
criarSlider("Rádio", 70, "Volume da rádio (SoundGroup 'Radio').", 310, function(v)
    local g = SoundService:FindFirstChild("Radio")
    if g then g.Volume = v / 100 end
end)
criarSlider("Armas", 70, "Volume das armas (SoundGroup 'Weapon').", 380, function(v)
    local g = SoundService:FindFirstChild("Weapon")
    if g then g.Volume = v / 100 end
end)
 
local outrosTitulo = Instance.new("TextLabel", scroll)
outrosTitulo.Text = "Outros"
outrosTitulo.Size = UDim2.new(1, 0, 0, 30)
outrosTitulo.Position = UDim2.new(0, 0, 0, 460)
outrosTitulo.BackgroundTransparency = 1
outrosTitulo.TextColor3 = Color3.new(1, 1, 1)
outrosTitulo.Font = Enum.Font.SourceSansBold
outrosTitulo.TextScaled = true
outrosTitulo.Parent = scroll
 
local autoJJActive = false
local anim = Instance.new("Animation")
anim.AnimationId = "rbxassetid://429681631"
 
criarBotaoOnOff("Auto_JJ's", "Faz polichinelo quando você digita no chat.", 500, function(state)
    autoJJActive = state
end)
 
TextChatService.SendingMessage:Connect(function()
    if autoJJActive then
        local char = player.Character or player.CharacterAdded:Wait()
        local humanoid = char:FindFirstChildOfClass("Humanoid")
        if humanoid then
            local track = humanoid:LoadAnimation(anim)
            track.Priority = Enum.AnimationPriority.Action
            track:Play()
            task.delay(0.5, function()
                if track and track.IsPlaying then
                    track:Stop()
                end
            end)
        end
    end
end)
 
gearButton.MouseButton1Click:Connect(function()
    configFrame.Visible = not configFrame.Visible
    blur.Enabled = configFrame.Visible
end)
closeBtn.MouseButton1Click:Connect(function()
    configFrame.Visible = false
    blur.Enabled = false
end)
