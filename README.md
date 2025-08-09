-- SERVIÇOS E VARIÁVEIS
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")
local Camera = workspace.CurrentCamera

local pos1, pos2, pos3, pos4 = nil, nil, nil, nil
local tweenEmExecucao = false
local teleguiadoVelocidade = 40

-- CORES PRETO E LARANJA
local corFundo = Color3.fromRGB(0, 0, 0)                -- Preto total
local corLaranja = Color3.fromRGB(255, 140, 0)          -- Laranja neon para texto
local corBordaBotao = Color3.fromRGB(255, 255, 255)     -- Borda branca
local corFundoBotao = Color3.fromRGB(20, 20, 20)        -- Botões preto escuro
local corBotaoHover = Color3.fromRGB(255, 120, 0)       -- Hover laranja

-- Função para criar borda branca estilo neon
local function criarNeon(frame)
    local neon = Instance.new("UIStroke")
    neon.Color = corBordaBotao
    neon.Thickness = 2
    neon.Transparency = 0
    neon.Parent = frame
end

-- GUI PRINCIPAL
local mainGui = Instance.new("ScreenGui", PlayerGui)
mainGui.Name = "LaGrandeCombinaison"
mainGui.ResetOnSpawn = false
mainGui.Enabled = true -- Já habilitado, sem key

local mainFrame = Instance.new("Frame", mainGui)
mainFrame.Size = UDim2.new(0, 300, 0, 330)
mainFrame.Position = UDim2.new(0.5, -150, 0.5, -165)
mainFrame.BackgroundColor3 = corFundo
mainFrame.Active = true
mainFrame.Draggable = true
Instance.new("UICorner", mainFrame).CornerRadius = UDim.new(0, 15)
criarNeon(mainFrame)

-- Ícone fechar (botão) "C" vermelho
local botaoFechar = Instance.new("TextButton", mainFrame)
botaoFechar.Size = UDim2.new(0, 30, 0, 30)
botaoFechar.Position = UDim2.new(1, -35, 0, 5)
botaoFechar.BackgroundColor3 = Color3.fromRGB(255, 0, 0) -- Vermelho forte
botaoFechar.Text = "C"
botaoFechar.TextColor3 = Color3.fromRGB(255, 255, 255)
botaoFechar.Font = Enum.Font.GothamBlack
botaoFechar.TextSize = 24
botaoFechar.AutoButtonColor = false
botaoFechar.ZIndex = 10
Instance.new("UICorner", botaoFechar).CornerRadius = UDim.new(0, 6)
criarNeon(botaoFechar)

botaoFechar.MouseEnter:Connect(function()
    botaoFechar.BackgroundColor3 = Color3.fromRGB(200, 0, 0) -- vermelho hover
end)
botaoFechar.MouseLeave:Connect(function()
    botaoFechar.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
end)

botaoFechar.MouseButton1Click:Connect(function()
    mainGui.Enabled = not mainGui.Enabled
end)

-- TÍTULO - Full Red e estilizado
local titulo = Instance.new("TextLabel", mainFrame)
titulo.Size = UDim2.new(1, -20, 0, 40)
titulo.Position = UDim2.new(0, 10, 0, 10)
titulo.BackgroundTransparency = 1
titulo.Text = "chicleteira bicicleteira hub"
titulo.Font = Enum.Font.GothamBlack
titulo.TextSize = 28
titulo.TextColor3 = Color3.fromRGB(255, 0, 0)           -- Vermelho forte
titulo.TextStrokeColor3 = Color3.fromRGB(150, 0, 0)    -- Contorno vermelho escuro
titulo.TextStrokeTransparency = 0
titulo.TextWrapped = true
titulo.TextXAlignment = Enum.TextXAlignment.Center

-- Subtítulo - Full Red suave
local subtitulo = Instance.new("TextLabel", mainFrame)
subtitulo.Size = UDim2.new(1, -20, 0, 25)
subtitulo.Position = UDim2.new(0, 10, 0, 52)
subtitulo.BackgroundTransparency = 1
subtitulo.Text = "Tiktok_Caio_clashv111"
subtitulo.Font = Enum.Font.GothamMedium
subtitulo.TextSize = 18
subtitulo.TextColor3 = Color3.fromRGB(255, 80, 80)      -- Vermelho mais claro
subtitulo.TextStrokeColor3 = Color3.fromRGB(150, 0, 0)
subtitulo.TextStrokeTransparency = 0
subtitulo.TextWrapped = true
subtitulo.TextXAlignment = Enum.TextXAlignment.Center

-- Criação de botões
local function criarBotao(texto, ordem)
    local b = Instance.new("TextButton", mainFrame)
    b.Size = UDim2.new(0, 280, 0, 40)
    b.Position = UDim2.new(0.5, -140, 0, 90 + (ordem - 1) * 45)
    b.Text = texto
    b.TextColor3 = corLaranja
    b.Font = Enum.Font.GothamBlack
    b.TextSize = 22
    b.AutoButtonColor = false
    b.BackgroundColor3 = corFundoBotao
    Instance.new("UICorner", b).CornerRadius = UDim.new(0, 12)

    -- Borda branca
    criarNeon(b)

    -- Botão hover (visual)
    b.MouseEnter:Connect(function() b.BackgroundColor3 = corBotaoHover end)
    b.MouseLeave:Connect(function() b.BackgroundColor3 = corFundoBotao end)
    return b
end

local botao1 = criarBotao("📍 Marcar posição 1", 1)
local botao2 = criarBotao("📍 Marcar posição 2", 2)
local botao3 = criarBotao("📍 Marcar posição 3", 3)
local botao4 = criarBotao("📍 Marcar posição 4", 4)
local botaoGo = criarBotao("🚀 Teleguiado", 5)

-- Lógica dos botões de posição
for i, botao in ipairs({botao1, botao2, botao3, botao4}) do
    botao.MouseButton1Click:Connect(function()
        local hrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            if i == 1 then pos1 = hrp.Position
            elseif i == 2 then pos2 = hrp.Position
            elseif i == 3 then pos3 = hrp.Position
            elseif i == 4 then pos4 = hrp.Position end

            botao.Text = "✅ Posição " .. i .. " marcada!"
            task.delay(2, function()
                botao.Text = "📍 Marcar posição " .. i
            end)
        end
    end)
end

-- Função pegar altura do chão para ajustar posição Y (não andar no ar)
local function pegarAlturaChao(pos)
    local raycastParams = RaycastParams.new()
    raycastParams.FilterDescendantsInstances = {player.Character}
    raycastParams.FilterType = Enum.RaycastFilterType.Blacklist
    local origem = Vector3.new(pos.X, pos.Y + 50, pos.Z)
    local resultado = workspace:Raycast(origem, Vector3.new(0, -100, 0), raycastParams)
    return resultado and resultado.Position.Y or pos.Y
end

-- Flag para bloquear movimento manual durante teleguiado
local bloqueadoParaTeleguiado = false

-- Função mover para posição (com tween e wait)
local function moverPara(destino, velocidade)
    if tweenEmExecucao or not destino then return end
    tweenEmExecucao = true

    local char = player.Character or player.CharacterAdded:Wait()
    local hrp = char:WaitForChild("HumanoidRootPart")
    local humanoid = char:WaitForChild("Humanoid")

    hrp:SetNetworkOwner(player)

    -- Ajusta Y para o chão + 2 de altura
    local yChao = pegarAlturaChao(destino)
    local destinoCorrigido = Vector3.new(destino.X, yChao + 2, destino.Z)

    local distancia = (hrp.Position - destinoCorrigido).Magnitude
    local tempo = distancia / velocidade

    local lookVector = (destinoCorrigido - hrp.Position).Unit
    local destinoCFrame = CFrame.new(destinoCorrigido, destinoCorrigido + lookVector)

    local tween = TweenService:Create(hrp, TweenInfo.new(tempo, Enum.EasingStyle.Linear), {CFrame = destinoCFrame})
    tween:Play()
    tween.Completed:Wait()

    -- Garante posição final e para velocidade/resfriamento
    hrp.CFrame = destinoCFrame
    hrp.Velocity = Vector3.zero
    hrp.RotVelocity = Vector3.zero

    tweenEmExecucao = false
end

-- Bloqueia controles do jogador (para anti-retorno)
local function bloquearControles()
    local humanoid = player.Character and player.Character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        humanoid.PlatformStand = true
    end
end

-- Libera controles do jogador
local function liberarControles()
    local humanoid = player.Character and player.Character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        humanoid.PlatformStand = false
    end
end

-- Botão teleguiado com sistema anti-retorno e anti-bug (sem Anchored)
botaoGo.MouseButton1Click:Connect(function()
    if tweenEmExecucao then return end
    local destinos = {pos1, pos2, pos3, pos4}

    -- Verifica se tem alguma posição marcada
    local temDestino = false
    for _, destino in ipairs(destinos) do
        if destino then
            temDestino = true
            break
        end
    end
    if not temDestino then return end

    local char = player.Character or player.CharacterAdded:Wait()
    local hrp = char:WaitForChild("HumanoidRootPart")

    bloqueadoParaTeleguiado = true
    bloquearControles()

    hrp:SetNetworkOwner(player)

    for _, destino in ipairs(destinos) do
        if destino then
            moverPara(destino, teleguiadoVelocidade)
            while tweenEmExecucao do task.wait() end
        end
    end

    liberarControles()
    bloqueadoParaTeleguiado = false

    hrp:SetNetworkOwner(nil)
end)

-- Event para impedir movimento manual fora do teleguiado
local function conectarAntiRetorno(humanoid)
    humanoid.Running:Connect(function(speed)
        if bloqueadoParaTeleguiado and speed > 0 then
            humanoid:Move(Vector3.new(0,0,0), false) -- bloqueia movimento manual
        end
    end)
end

if player.Character then
    local humanoid = player.Character:FindFirstChildOfClass("Humanoid")
    if humanoid then
        conectarAntiRetorno(humanoid)
    end
end

player.CharacterAdded:Connect(function(char)
    local humanoid = char:WaitForChild("Humanoid")
    conectarAntiRetorno(humanoid)
end)

-- Funções anti-morte e anti-reset, limpeza de bugs visuais

local function ativarAntiResetEMorte(humanoid, hrp)
    task.spawn(function()
        humanoid.HealthChanged:Connect(function()
            if humanoid.Health < humanoid.MaxHealth then
                humanoid.Health = humanoid.MaxHealth
            end
        end)
        while humanoid.Parent do
            humanoid:SetStateEnabled(Enum.HumanoidStateType.Dead, false)
            if humanoid.Health < humanoid.MaxHealth then
                humanoid.Health = humanoid.MaxHealth
            end
            task.wait(0.3)
        end
    end)

    humanoid.Died:Connect(function()
        task.wait(0.5)
        local newChar = player.Character or player.CharacterAdded:Wait()
        local newHrp = newChar:WaitForChild("HumanoidRootPart")
        local newHumanoid = newChar:WaitForChild("Humanoid")
        ativarAntiResetEMorte(newHumanoid, newHrp)

        if pos1 then
            newHrp.CFrame = CFrame.new(pos1 + Vector3.new(0, 3, 0))
        end
    end)
end

local function limparBugsVisuais(char)
    local hrp = char:WaitForChild("HumanoidRootPart")
    task.spawn(function()
        while hrp and hrp.Parent do
            for _, part in pairs(char:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.Transparency = 0
                    part.CanCollide = true
                    part.Anchored = false
                    if part:IsA("MeshPart") then
                        part.LocalTransparencyModifier = 0
                    end
                    if (part.Position - hrp.Position).Magnitude > 7 and part.Name ~= "HumanoidRootPart" then
                        pcall(function() part:Destroy() end)
                    end
                elseif part:IsA("Decal") then
                    part.Transparency = 0
                end
            end
            task.wait(1)
        end
    end)
end

player.CharacterAdded:Connect(function(char)
    local humanoid = char:WaitForChild("Humanoid")
    local hrp = char:WaitForChild("HumanoidRootPart")

    limparBugsVisuais(char)
    ativarAntiResetEMorte(humanoid, hrp)
end)

if player.Character then
    local char = player.Character
    local humanoid = char:FindFirstChildOfClass("Humanoid")
    local hrp = char:FindFirstChild("HumanoidRootPart")
    if humanoid and hrp then
        limparBugsVisuais(char)
        ativarAntiResetEMorte(humanoid, hrp)
    end
end
