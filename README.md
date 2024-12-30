-- Script que exibe o nome dos outros jogadores acima das cabeças, com cor branca

-- Obtém o jogador local (o jogador que está executando o script)
local jogadorLocal = game.Players.LocalPlayer

-- Função que cria um BillboardGui para mostrar o nome do jogador
local function criarNomeAcimaDaCabeca(jogador)
    -- Cria um BillboardGui
    local billboardGui = Instance.new("BillboardGui")
    billboardGui.Parent = jogador.Character:WaitForChild("Head") -- Posiciona no topo da cabeça do jogador
    billboardGui.Adornee = jogador.Character:WaitForChild("Head") -- A BillboardGui vai seguir a cabeça
    billboardGui.Size = UDim2.new(0, 200, 0, 50) -- Tamanho do BillboardGui
    billboardGui.StudsOffset = Vector3.new(0, 3, 0) -- Distância vertical da cabeça (ajustável)

    -- Cria um TextLabel dentro do BillboardGui
    local nomeLabel = Instance.new("TextLabel")
    nomeLabel.Parent = billboardGui
    nomeLabel.Text = jogador.Name
    nomeLabel.TextColor3 = Color3.fromRGB(255, 255, 255) -- Cor branca
    nomeLabel.TextSize = 24 -- Tamanho da fonte
    nomeLabel.BackgroundTransparency = 1 -- Sem fundo
    nomeLabel.TextScaled = true
end

-- Função que exibe os nomes dos jogadores acima das cabeças
local function mostrarNomesDosJogadores()
    -- Itera sobre todos os jogadores no jogo
    for _, jogador in pairs(game.Players:GetPlayers()) do
        -- Ignora o jogador local (não mostra o nome do jogador que está executando o script)
        if jogador ~= jogadorLocal then
            -- Cria o nome acima da cabeça do jogador
            if jogador.Character and jogador.Character:FindFirstChild("Head") then
                criarNomeAcimaDaCabeca(jogador)
            end
        end
    end
end

-- Chama a função para mostrar os nomes dos outros jogadores
mostrarNomesDosJogadores()

-- Observa se um jogador entra no jogo para adicionar o nome automaticamente
game.Players.PlayerAdded:Connect(function(jogador)
    if jogador ~= jogadorLocal then
        -- Aguarda o jogador ter uma cabeça para poder criar o nome
        jogador.CharacterAdded:Connect(function()
            if jogador.Character and jogador.Character:FindFirstChild("Head") then
                criarNomeAcimaDaCabeca(jogador)
            end
        end)
    end
end)

-- Observa se um jogador sai do jogo e remove o nome
game.Players.PlayerRemoving:Connect(function(jogador)
    for _, gui in pairs(jogador.Character:GetChildren()) do
        if gui:IsA("BillboardGui") then
            gui:Destroy()
        end
    end
end)
