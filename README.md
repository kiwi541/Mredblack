local teleportDistance = 5  -- Distância para teleportar ao redor do jogador
local teleportSpeed = 200   -- Velocidade do teleporte
local isTeleporting = true  -- Variável para controlar o teleporte

local Gui = Instance.new("ScreenGui")
local TextBox = Instance.new("TextBox")
local TestButton = Instance.new("TextButton")
local DraggableFrame = Instance.new("Frame")

-- Configuração do GUI
Gui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
Gui.Name = "MyGui"

-- Configuração do Frame arrastável
DraggableFrame.Parent = Gui
DraggableFrame.Size = UDim2.new(0, 300, 0, 150)
DraggableFrame.Position = UDim2.new(0.5, -150, 0.5, -75)
DraggableFrame.BackgroundColor3 = Color3.fromRGB(139, 69, 19)  -- Cor marrom
DraggableFrame.Active = true
DraggableFrame.Draggable = true

-- Configuração do TextBox
TextBox.Parent = DraggableFrame
TextBox.Size = UDim2.new(0, 200, 0, 50)
TextBox.Position = UDim2.new(0.5, -100, 0.2, 0)
TextBox.BackgroundColor3 = Color3.fromRGB(139, 69, 19)  -- Cor marrom
TextBox.TextColor3 = Color3.fromRGB(0, 255, 0)  -- Texto verde
TextBox.PlaceholderText = "Digite parte do nome"
TextBox.Name = "PlayerNickBox"

-- Configuração do botão "Teste"
TestButton.Parent = DraggableFrame
TestButton.Size = UDim2.new(0, 200, 0, 50)
TestButton.Position = UDim2.new(0.5, -100, 0.6, 0)
TestButton.BackgroundColor3 = Color3.fromRGB(139, 69, 19)  -- Cor marrom
TestButton.TextColor3 = Color3.fromRGB(0, 255, 0)  -- Texto verde
TestButton.Text = "Teste"
TestButton.Name = "TestButton"

-- Função para teleportar ao redor do jogador
local function teleportAroundPlayer(player)
    while isTeleporting do
        if player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local humanoidRootPart = player.Character.HumanoidRootPart
            local randomAngle = math.random() * 2 * math.pi
            local offsetX = math.cos(randomAngle) * teleportDistance
            local offsetZ = math.sin(randomAngle) * teleportDistance
            game.Players.LocalPlayer.Character:SetPrimaryPartCFrame(CFrame.new(humanoidRootPart.Position + Vector3.new(offsetX, 0, offsetZ)))
        end
        wait(1/teleportSpeed)  -- Ajuste do intervalo para corresponder à velocidade do teleporte
    end
end

-- Evento para o botão "Teste"
TestButton.MouseButton1Click:Connect(function()
    local player = game.Players:FindFirstChild(TextBox.Text)
    for _, p in ipairs(game.Players:GetPlayers()) do
        if string.find(string.lower(p.Name), string.lower(TextBox.Text)) then
            player = p
            break
        end
    end
    if player then
        if isTeleporting then
            isTeleporting = false
            TestButton.Text = "Teste"
        else
            isTeleporting = true
            TestButton.Text = "Parar"
            teleportAroundPlayer(player)
        end
    else
        warn("Jogador não encontrado!")
    end
end)

-- Evento para parar o teleporte ao morrer
game.Players.LocalPlayer.Character.Humanoid.Died:Connect(function()
    isTeleporting = false
    TestButton.Text = "Teste"
end)
