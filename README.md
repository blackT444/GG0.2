local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Workspace = game:GetService("Workspace")

-- Criar GUI principal
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)

-- Criar botão "P"
local PButton = Instance.new("TextButton", ScreenGui)
PButton.Size = UDim2.new(0, 50, 0, 50)
PButton.Position = UDim2.new(0.05, 0, 0.85, 0)
PButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
PButton.Text = "P"
PButton.TextScaled = true
PButton.TextColor3 = Color3.new(1, 1, 1)
PButton.BackgroundTransparency = 0.3
PButton.BorderSizePixel = 0
PButton.Font = Enum.Font.SourceSansBold

-- Criar UICorner para tornar o botão "P" circular
local UICornerP = Instance.new("UICorner", PButton)
UICornerP.CornerRadius = UDim.new(1, 0)

-- Criar menu de jogadores
local PlayerListFrame = Instance.new("Frame", ScreenGui)
PlayerListFrame.Size = UDim2.new(0, 200, 0, 300)
PlayerListFrame.Position = UDim2.new(0.05, 0, 0.55, 0)
PlayerListFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
PlayerListFrame.Visible = false
PlayerListFrame.BorderSizePixel = 0

-- Criar ScrollingFrame para rolagem da lista de jogadores
local ScrollFrame = Instance.new("ScrollingFrame", PlayerListFrame)
ScrollFrame.Size = UDim2.new(1, 0, 1, -40)
ScrollFrame.CanvasSize = UDim2.new(0, 0, 5, 0)
ScrollFrame.BackgroundTransparency = 1
ScrollFrame.ScrollBarThickness = 5

-- Criar UIListLayout para organizar os botões
local UIListLayout = Instance.new("UIListLayout", ScrollFrame)
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder

-- Criar botão "T" para ativar o efeito
local TButton = Instance.new("TextButton", ScreenGui)
TButton.Size = UDim2.new(0, 50, 0, 50)
TButton.Position = UDim2.new(0.12, 0, 0.85, 0)
TButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
TButton.Text = "T"
TButton.TextScaled = true
TButton.TextColor3 = Color3.new(1, 1, 1)
TButton.BackgroundTransparency = 0.3
TButton.BorderSizePixel = 0
TButton.Font = Enum.Font.SourceSansBold
TButton.Visible = false -- Oculto até escolher jogador

-- Criar UICorner para tornar o botão "T" circular
local UICornerT = Instance.new("UICorner", TButton)
UICornerT.CornerRadius = UDim.new(1, 0)

local SelectedTarget = nil -- Armazena o jogador escolhido

-- Função para abrir/fechar menu de jogadores
PButton.MouseButton1Click:Connect(function()
    PlayerListFrame.Visible = not PlayerListFrame.Visible
    TButton.Visible = false
    SelectedTarget = nil -- Reseta seleção

    -- Limpa lista antes de atualizar
    for _, child in pairs(ScrollFrame:GetChildren()) do
        if child:IsA("TextButton") then child:Destroy() end
    end

    -- Atualiza lista de jogadores
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            local PlayerButton = Instance.new("TextButton", ScrollFrame)
            PlayerButton.Size = UDim2.new(1, 0, 0, 40)
            PlayerButton.Text = player.Name
            PlayerButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
            PlayerButton.TextColor3 = Color3.new(1, 1, 1)
            PlayerButton.BorderSizePixel = 0

            -- Selecionar jogador e ativar botão "T"
            PlayerButton.MouseButton1Click:Connect(function()
                SelectedTarget = player
                TButton.Visible = true
                PlayerListFrame.Visible = false
            end)
        end
    end
end)

-- Função para fazer o jogador atravessar o chão
local function MakePlayerFall(Target)
    if Target and Target.Character then
        for _, part in pairs(Target.Character:GetChildren()) do
            if part:IsA("BasePart") then
                part.CanCollide = false -- Desativa colisão do jogador
            end
        end
    end
end

-- Atacar jogador ao clicar no botão "T"
TButton.MouseButton1Click:Connect(function()
    if SelectedTarget then
        MakePlayerFall(SelectedTarget)
    end
end)
