jiloadstring(game:HttpGet("https://pastebin.com/raw/bGHGPcT1"))()

local args = {
            [1] = "PickingTools",
            [2] = "Couch"
        }
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))

local OrionLib = loadstring(game:HttpGet("https://pastebin.com/raw/0YuQQ9WS"))()
local Window = OrionLib:MakeWindow({Name = "Segaz PremiumðŸ©¸", HidePremium = false, IntroText = "Studio Segaz ðŸ‡§ðŸ‡·", SaveConfig = true, ConfigFolder = "Studio Igor"})

-- Lista de seleÃ§Ã£o de jogadores para "Goto"
local gotoPlayerList = {}
local selectedGotoPlayer = nil
local avisoToggle = false

local function updatePlayerList()
gotoPlayerList = {}
for _, player in ipairs(game.Players:GetPlayers()) do
table.insert(gotoPlayerList, player.Name)
end
end

updatePlayerList()

-- Adicionar um ListPlayer para selecionar o jogador alvo para "Goto"
local Tab = Window:MakeTab({
Name = "Players | Brookhaven ",
Icon = "rbxassetid://414904019",
PremiumOnly = false
})

-- Add buttons to the Flings tab
Tab:AddButton({
    Name = "Pegar sofa (OBRIGATORIO)",
    Description = "E Obrigatorio o Uso do sofa pra o kill",
    Callback = function()
        local args = {
            [1] = "PickingTools",
            [2] = "Couch"
        }
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))
        print('Hello!')
    end
})

Tab:AddDropdown({
Name = "Lista de Jogadores",
Description = "Selecione o jogador alvo para o Goto (couch)",
Options = gotoPlayerList,
Callback = function(playerName)
selectedGotoPlayer = playerName
end
})

-- Adicionar botÃ£o para resetar a lista de jogadores
Tab:AddButton({
Name = "Reset Player List",
Callback = function()
updatePlayerList()
playerDropdown:Refresh(gotoPlayerList, true)
end
})

-- Adicionar toggle para view
Tab:AddToggle({
Name = "View",
Default = false,
Callback = function(state)
viewToggle = state
if viewToggle and selectedGotoPlayer then
local player = game.Players:FindFirstChild(selectedGotoPlayer)
if player then
game.Workspace.CurrentCamera.CameraSubject = player.Character.Humanoid
else
print("Jogador nÃ£o encontrado.")
end
else
game.Workspace.CurrentCamera.CameraSubject = game.Players.LocalPlayer.Character.Humanoid
end
end
})

-- Adicionar toggle para follow
Tab:AddToggle({
Name = "Follow",
Default = false,
Callback = function(state)
followToggle = state
while followToggle do
if selectedGotoPlayer then
local player = game.Players:FindFirstChild(selectedGotoPlayer)
if player then
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = player.Character.HumanoidRootPart.CFrame
else
print("Jogador nÃ£o encontrado.")
end
end
wait(0.1)
end
end
})

-- Adicionar o botÃ£o "Goto" Ã  seÃ§Ã£o "View/Goto"
Tab:AddButton({
Name = "Goto",
Description = "This player is not on the list",
Callback = function()
if selectedGotoPlayer then
local player = game.Players:FindFirstChild(selectedGotoPlayer)
if player then
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = player.Character.HumanoidRootPart.CFrame
else
print("Jogador nÃ£o encontrado.")
end
else
print("Nenhum jogador selecionado para o Goto.")
end
end
})

-- Conectar eventos de jogador removido
game.Players.PlayerRemoving:Connect(function(player)
updatePlayerList()
if avisoToggle then
OrionLib:MakeNotification({
Name = "Aviso",
Content = player.Name .. " saiu do jogo",
Image = "rbxassetid://4483345998",
Time = 5
})
end
end)

-- Conectar eventos de jogador adicionado
game.Players.PlayerAdded:Connect(function(player)
updatePlayerList()
if avisoToggle then
OrionLib:MakeNotification({
Name = "Aviso",
Content = player.Name .. " entrou no jogo",
Image = "rbxassetid://4483345998",
Time = 5
})
end
end)

-- FunÃ§Ã£o para manter a lista de jogadores atualizada
local function maintainPlayerList()
while wait(1) do
updatePlayerList()
end
end

-- Iniciar a funÃ§Ã£o de manutenÃ§Ã£o da lista de jogadores
spawn(maintainPlayerList)

-- Adicionar toggle para avisos
Tab:AddToggle({
Name = "Avisos",
Default = false,
Callback = function(state)
avisoToggle = state
end
})

local Section = Tab:AddSection({
Name = "MATAR JOGADOR"
})
local selectedKillAdvancedPlayer = nil
local couchEquipped = false

local function killAdvancedPlayer()
if selectedKillAdvancedPlayer then
local player = game.Players:FindFirstChild(selectedKillAdvancedPlayer)
if player then
-- Equipa o item 'Couch' no inventÃ¡rio se ainda nÃ£o estiver equipado
local backpack = game.Players.LocalPlayer.Backpack
if backpack and not couchEquipped then
local couch = backpack:FindFirstChild("Couch")
if couch then
game.Players.LocalPlayer.Character.Humanoid:EquipTool(couch)
couchEquipped = true
else
print("O item 'Couch' nÃ£o foi encontrado no seu inventÃ¡rio.")
end
end

-- Looping de teleportes no jogador selecionado da lista
while true do
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = player.Character.HumanoidRootPart.CFrame
wait(0.0) -- Intervalo entre cada teleporte, ajuste conforme necessÃ¡rio

-- Verifica se o jogador sentou no 'Couch' e realiza o teleporte para o cÃ©u
if player.Character:FindFirstChild("Humanoid") and player.Character.Humanoid.SeatPart then
player.Character.HumanoidRootPart.CFrame = CFrame.new(0, 0, 0) -- Teleporta para cima
wait(0.0) -- Espera um pouco antes de teleportar de volta para evitar bugs
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(0, 0, 0) -- Teleporta para cima novamente
wait(0.0) -- Espera um pouco antes de teleportar de volta para evitar bugs
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(649.78, -439.87, 126.35) -- Teleporta de volta para a posiÃ§Ã£o original
break -- Sai do loop apÃ³s teleportar de volta
end
end

-- Remove o item 'Couch' da mÃ£o do jogador apÃ³s o teleporte para o cÃ©u
if couchEquipped then
local backpack = game.Players.LocalPlayer.Backpack
if backpack then
local couch = backpack:FindFirstChild("Couch")
if couch then
couch.Parent = nil -- Remove o 'Couch' do inventÃ¡rio
couchEquipped = false
end
end
end
else
print("Jogador nÃ£o encontrado.")
end
else
print("Nenhum jogador selecionado para o Bring AvanÃ§ado.")
end
end

-- Lista de Players para Bring AvanÃ§ado
local killAdvancedPlayerList = {}
for _, player in ipairs(game.Players:GetPlayers()) do
table.insert(killAdvancedPlayerList, player.Name)
end

Tab:AddDropdown({
Name = "Selecionar Jogador",
Description = "Selecione o jogador alvo para o Bring AvanÃ§ado",
Options = killAdvancedPlayerList,
Callback = function(playerName)
selectedKillAdvancedPlayer = playerName
end
})

Tab:AddButton({
Name = "KILL (ative A animacÃ£o Deite-se)",
Description = "Equipa o item 'Couch' e teleporta o jogador selecionado",
Callback = function()
killAdvancedPlayer()
end
})

Tab:AddButton({
Name = "Puxar Todos (Trollar)",
     Callback = function()
     --KK
     
     local args = {
    [1] = "ClearAllTools"
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Clea1rTool1s"):FireServer(unpack(args))

	--Couch Item
	
	local args = {
    [1] = "PickingTools",
    [2] = "Couch"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))

--Fling aall

loadstring(game:HttpGet("https://pastebin.com/raw/zqyDSUWX"))()
   end
})

local Section = Tab:AddSection({
Name = "Kill V2 [!] PODE CONTER ERROS"
})


-- ServiÃ§os necessÃ¡rios
local playerService = game:GetService('Players')
local runService = game:GetService('RunService')
local localPlayer = playerService.LocalPlayer
local backpack = localPlayer:FindFirstChildOfClass('Backpack')

-- VariÃ¡veis globais
local flingV14Toggle = false
local selectedFlingPlayerV14 = nil
local flingV14Connection
local playerSpawnedConnection
local spinFlingConnection
local bodyThrust

-- FunÃ§Ã£o para obter a lista de jogadores
local function getPlayerList()
    local playerList = {}
    for _, player in ipairs(playerService:GetPlayers()) do
        if player ~= localPlayer then
            table.insert(playerList, player.Name)
        end
    end
    return playerList
end

-- FunÃ§Ã£o para atualizar o dropdown
local function updateDropdown(dropdown)
    UpdateDropdown(dropdown, getPlayerList())
end

-- FunÃ§Ã£o para equipar o item "Couch"
local function equipCouch()
    print("Tentando equipar o sofÃ¡...")
    if backpack then
        local couchItem = backpack:FindFirstChild("Couch")
        if couchItem then
            couchItem.Parent = localPlayer.Character
            local humanoid = localPlayer.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid:EquipTool(couchItem)
                print("SofÃ¡ equipado.")
            else
                print("Humanoide nÃ£o encontrado.")
            end
        else
            warn("SofÃ¡ nÃ£o encontrado na mochila.")
        end
    else
        print("Mochila nÃ£o encontrada.")
    end
end

-- FunÃ§Ã£o para teleportar para a coordenada
local function teleportToCoordinate()
    print("Teleportando para a coordenada...")
    local teleportPosition = Vector3.new(-499.00, -783.95, 239.72) -- Coordenada para onde vocÃª deseja teleportar
    localPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(teleportPosition)
end

-- FunÃ§Ã£o para flingar jogador (V14)
local function flingV14(targetPlayerName)
    local targetPlayer = playerService:FindFirstChild(targetPlayerName)
    if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        -- Looping de teleportes no jogador selecionado
        flingV14Connection = runService.Heartbeat:Connect(function()
            if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                local targetPosition = targetPlayer.Character.HumanoidRootPart.CFrame
                      if toggle then
                    -- Teleportando uma unidade para cima
                    localPlayer.Character.HumanoidRootPart.CFrame = targetPosition * CFrame.new(0, 2, 0)
                else
                    -- Teleportando uma unidade para baixo
                    localPlayer.Character.HumanoidRootPart.CFrame = targetPosition * CFrame.new(0, -1, 0)
                end
                toggle = not toggle
            end

            -- Verifica se o jogador sentou no 'SeatCouch' e realiza o teleporte para a coordenada
            if targetPlayer.Character:FindFirstChild("Humanoid") and targetPlayer.Character.Humanoid.SeatPart then
                teleportToCoordinate()
                flingV14Connection:Disconnect()
                flingV14Connection = nil
            end
        end)
    else
        print("Jogador alvo nÃ£o encontrado ou invÃ¡lido.")
    end
end

-- FunÃ§Ã£o para detectar quando o jogador renasce
local function onPlayerRespawned(targetPlayer)
    if flingV14Toggle then
        -- Aguardar o renascimento do jogador
        playerSpawnedConnection = targetPlayer.CharacterAdded:Connect(function()
            wait(0.0) -- Tempo para garantir que o personagem foi totalmente carregado
            if flingV14Toggle then
                equipCouch() -- Equipar o sofÃ¡ quando o jogador renasce
                flingV14(targetPlayer.Name)
            end
        end)
    end
end

-- FunÃ§Ã£o para verificar e equipar o sofÃ¡ caso nÃ£o esteja equipado
local function ensureCouchEquipped()
    local character = localPlayer.Character
    if character then
        local equipped = false
        for _, tool in ipairs(character:GetChildren()) do
            if tool:IsA("Tool") and tool.Name == "Couch" then
                equipped = true
                break
            end
        end
        if not equipped then
            equipCouch()
        end
    else
        print("Personagem nÃ£o encontrado.")
    end
end

-- FunÃ§Ã£o para desativar as colisÃµes do personagem
local function disableCollisions(character)
    if character and character:FindFirstChild("Head") and character:FindFirstChild("UpperTorso") and character:FindFirstChild("LowerTorso") and character:FindFirstChild("HumanoidRootPart") then
        character.Head.CanCollide = false
        character.UpperTorso.CanCollide = false
        character.LowerTorso.CanCollide = false
        character.HumanoidRootPart.CanCollide = false
    else
        print("Parte do personagem nÃ£o encontrada.")
    end
end

-- FunÃ§Ã£o para reativar as colisÃµes do personagem
local function resetCollisions(character)
    if character and character:FindFirstChild("Head") and character:FindFirstChild("UpperTorso") and character:FindFirstChild("LowerTorso") and character:FindFirstChild("HumanoidRootPart") then
        character.Head.CanCollide = true
        character.UpperTorso.CanCollide = true
        character.LowerTorso.CanCollide = true
        character.HumanoidRootPart.CanCollide = true
    end
end

-- FunÃ§Ã£o para ativar o Spin Fling
local function activateSpinFling()
    local character = localPlayer.Character or localPlayer.CharacterAdded:Wait()
    local power = 1500 -- Pode ajustar isso para alterar a forÃ§a do impulso

    -- Cria um novo BodyThrust
    bodyThrust = Instance.new("BodyThrust")
    bodyThrust.Parent = character.HumanoidRootPart
    bodyThrust.Force = Vector3.new(power, 1100, power)
    bodyThrust.Location = character.HumanoidRootPart.Position

    -- Desativa as colisÃµes
    spinFlingConnection = runService.Stepped:Connect(function()
        disableCollisions(character)
    end)

    print("Spin Fling ativado!")
end

-- FunÃ§Ã£o para desativar o Spin Fling
local function deactivateSpinFling()
    local character = localPlayer.Character or localPlayer.CharacterAdded:Wait()

    -- Desconecta a funÃ§Ã£o de desativaÃ§Ã£o de colisÃµes
    if spinFlingConnection then
        spinFlingConnection:Disconnect()
        spinFlingConnection = nil
    end

    -- Remove o efeito de impulso do corpo
    if bodyThrust then
        bodyThrust:Destroy()
        bodyThrust = nil
    end

    -- Restaura as colisÃµes do personagem
    resetCollisions(character)

    print("Spin Fling desativado!")
end

-- FunÃ§Ã£o de callback para o toggle
local function onFlingV14Toggle(value)
    print("Toggle Fling V14:", value)
    flingV14Toggle = value
    if flingV14Toggle and selectedFlingPlayerV14 then
        local targetPlayer = playerService:FindFirstChild(selectedFlingPlayerV14)
        if targetPlayer then
            ensureCouchEquipped() -- Equipar o sofÃ¡ ao ativar o toggle
            flingV14(targetPlayer.Name)
            onPlayerRespawned(targetPlayer)
            activateSpinFling() -- Ativar o Spin Fling quando o toggle Ã© ativado
        end
    elseif not flingV14Toggle then
        -- Desconecta as conexÃµes quando o toggle Ã© desativado
        if flingV14Connection then
            flingV14Connection:Disconnect()
            flingV14Connection = nil
        end
        if playerSpawnedConnection then
            playerSpawnedConnection:Disconnect()
            playerSpawnedConnection = nil
        end
        deactivateSpinFling() -- Desativar o Spin Fling quando o toggle Ã© desativado
    end
end

-- Dropdown para selecionar o jogador
Tab:AddDropdown({
    Name = "Selecione o Jogador para Fling",
    Options = getPlayerList(),
    Default = "",
    Callback = function(playerName)
        selectedFlingPlayerV14 = playerName
        print("Jogador selecionado para Fling:", playerName)
    end
})

-- Atualiza a lista de jogadores quando jogadores entram ou saem do jogo
playerService.PlayerAdded:Connect(function()
    print("Novo jogador adicionado. Atualizando a lista...")
    updateDropdown(PlayerDropdown)
end)

playerService.PlayerRemoving:Connect(function()
    print("Jogador saiu. Atualizando a lista...")
    updateDropdown(PlayerDropdown)
end)

-- Toggle para ativar/desativar o Fling
Tab:AddToggle({
    Name = "Matar Jogador Selecionado!",
    Default = false,
    Callback = function(value)
        print("Toggle Fling V14:", value)
        flingV14Toggle = value
        if flingV14Toggle and selectedFlingPlayerV14 then
            local targetPlayer = playerService:FindFirstChild(selectedFlingPlayerV14)
            if targetPlayer then
                ensureCouchEquipped() -- Equipar o sofÃ¡ ao ativar o toggle
                flingV14(targetPlayer.Name)
                onPlayerRespawned(targetPlayer)
                activateSpinFling() -- Ativar o Spin Fling quando o toggle Ã© ativado
            end
        elseif not flingV14Toggle then
            -- Desconecta as conexÃµes quando o toggle Ã© desativado
            if flingV14Connection then
                flingV14Connection:Disconnect()
                flingV14Connection = nil
            end
            if playerSpawnedConnection then
                playerSpawnedConnection:Disconnect()
                playerSpawnedConnection = nil
            end
            deactivateSpinFling() -- Desativar o Spin Fling quando o toggle Ã© desativado
        end
    end
})

local avTab = Window:MakeTab({
Name = "Avatar | Brookhaven",
Icon = "rbxassetid://414904019",
PremiumOnly = false
})

avTab:AddToggle({
Name = "Mudar de cor Automaticamente",
Default = false,
Callback = function(value)
isActive = value
while isActive do
local colors = {"Eggplant", "Medium stone grey", "Dark nougat", "Really red", "New Yeller", "Lime green", "Toothpaste", "Dirt brown"}
for _, color in ipairs(colors) do
local args = {
[1] = "skintone",
[2] = color
}
game:GetService("ReplicatedStorage").RE:FindFirstChild("1Updat1eAvata1r"):FireServer(unpack(args))
wait(1) -- espera 1 segundo entre as execuÃ§Ãµes
end
end
end
})

avTab:AddButton({
Name = "Virar PÃ³ Brilhante",
     Callback = function()
     --Size
     
     local args = {
    [1] = "CharacterSizeDown",
    [2] = 4
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Clothe1s"):FireServer(unpack(args))

--Item

local args = {
    [1] = "wear",
    [2] = 173624651
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Updat1eAvata1r"):FireServer(unpack(args))

--outro

local args = {
    [1] = "wear",
    [2] = 141742418
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Updat1eAvata1r"):FireServer(unpack(args))
end
})
local Section = avTab:AddSection({
	Name = "Extras"
})
avTab:AddButton({
Name = "Virar Assasino",
	Callback = function()
	--Assain

	local args = {
    [1] = "wear",
    [2] = 15133320948
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Updat1eAvata1r"):FireServer(unpack(args))
end
})
avTab:AddButton({
Name = "Babiromet do davi scripts",
	Callback = function()
	--Assain

	local args = {
    [1] = "CharacterChange",
    [2] = {
        [1] = 14731377941,
        [2] = 14731377894,
        [3] = 14731377875,
        [4] = 14731384498,
        [5] = 14731377938,
        [6] = 0
    },
    [3] = "Stick Bug"
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Avata1rOrigina1l"):FireServer(unpack(args))

--Man

local args = {
    [1] = "wear",
    [2] = 6564572490
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Updat1eAvata1r"):FireServer(unpack(args))

--speed

game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 90
end
})
avTab:AddButton({
Name = "Virar Esqueleto (F.E)",
     Callback = function()
     local args = {
    [1] = "CharacterChange",
    [2] = {
        [1] = 36781360,
        [2] = 36781407,
        [3] = 36781447,
        [4] = 36781481,
        [5] = 36781518,
        [6] = 0
    },
    [3] = "Skeleton"
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Avata1rOrigina1l"):FireServer(unpack(args))
end
})

local gtTab = Window:MakeTab({
Name = "Itens | Brookhaven",
Icon = "rbxassetid://414904019",
PremiumOnly = false
})

local function AddButton(name, tool)
    gtTab:AddButton({
        Name = name,
        Callback = function()
            local args = {
                [1] = "PickingTools",
                [2] = tool
            }
            local remoteFunction = game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l")
            if remoteFunction theloadstring(game:HttpGet("https://pastebin.com/raw/bGHGPcT1"))()

local args = {
            [1] = "PickingTools",
            [2] = "Couch"
        }
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))

local OrionLib = loadstring(game:HttpGet("https://pastebin.com/raw/0YuQQ9WS"))()
local Window = OrionLib:MakeWindow({Name = "Segaz PremiumðŸ©¸", HidePremium = false, IntroText = "Studio Segaz ðŸ‡§ðŸ‡·", SaveConfig = true, ConfigFolder = "Studio Igor"})

-- Lista de seleÃ§Ã£o de jogadores para "Goto"
local gotoPlayerList = {}
local selectedGotoPlayer = nil
local avisoToggle = false

local function updatePlayerList()
gotoPlayerList = {}
for _, player in ipairs(game.Players:GetPlayers()) do
table.insert(gotoPlayerList, player.Name)
end
end

updatePlayerList()

-- Adicionar um ListPlayer para selecionar o jogador alvo para "Goto"
local Tab = Window:MakeTab({
Name = "Players | Brookhaven ",
Icon = "rbxassetid://414904019",
PremiumOnly = false
})

-- Add buttons to the Flings tab
Tab:AddButton({
    Name = "Pegar sofa (OBRIGATORIO)",
    Description = "E Obrigatorio o Uso do sofa pra o kill",
    Callback = function()
        local args = {
            [1] = "PickingTools",
            [2] = "Couch"
        }
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))
        print('Hello!')
    end
})

Tab:AddDropdown({
Name = "Lista de Jogadores",
Description = "Selecione o jogador alvo para o Goto (couch)",
Options = gotoPlayerList,
Callback = function(playerName)
selectedGotoPlayer = playerName
end
})

-- Adicionar botÃ£o para resetar a lista de jogadores
Tab:AddButton({
Name = "Reset Player List",
Callback = function()
updatePlayerList()
playerDropdown:Refresh(gotoPlayerList, true)
end
})

-- Adicionar toggle para view
Tab:AddToggle({
Name = "View",
Default = false,
Callback = function(state)
viewToggle = state
if viewToggle and selectedGotoPlayer then
local player = game.Players:FindFirstChild(selectedGotoPlayer)
if player then
game.Workspace.CurrentCamera.CameraSubject = player.Character.Humanoid
else
print("Jogador nÃ£o encontrado.")
end
else
game.Workspace.CurrentCamera.CameraSubject = game.Players.LocalPlayer.Character.Humanoid
end
end
})

-- Adicionar toggle para follow
Tab:AddToggle({
Name = "Follow",
Default = false,
Callback = function(state)
followToggle = state
while followToggle do
if selectedGotoPlayer then
local player = game.Players:FindFirstChild(selectedGotoPlayer)
if player then
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = player.Character.HumanoidRootPart.CFrame
else
print("Jogador nÃ£o encontrado.")
end
end
wait(0.1)
end
end
})

-- Adicionar o botÃ£o "Goto" Ã  seÃ§Ã£o "View/Goto"
Tab:AddButton({
Name = "Goto",
Description = "This player is not on the list",
Callback = function()
if selectedGotoPlayer then
local player = game.Players:FindFirstChild(selectedGotoPlayer)
if player then
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = player.Character.HumanoidRootPart.CFrame
else
print("Jogador nÃ£o encontrado.")
end
else
print("Nenhum jogador selecionado para o Goto.")
end
end
})

-- Conectar eventos de jogador removido
game.Players.PlayerRemoving:Connect(function(player)
updatePlayerList()
if avisoToggle then
OrionLib:MakeNotification({
Name = "Aviso",
Content = player.Name .. " saiu do jogo",
Image = "rbxassetid://4483345998",
Time = 5
})
end
end)

-- Conectar eventos de jogador adicionado
game.Players.PlayerAdded:Connect(function(player)
updatePlayerList()
if avisoToggle then
OrionLib:MakeNotification({
Name = "Aviso",
Content = player.Name .. " entrou no jogo",
Image = "rbxassetid://4483345998",
Time = 5
})
end
end)

-- FunÃ§Ã£o para manter a lista de jogadores atualizada
local function maintainPlayerList()
while wait(1) do
updatePlayerList()
end
end

-- Iniciar a funÃ§Ã£o de manutenÃ§Ã£o da lista de jogadores
spawn(maintainPlayerList)

-- Adicionar toggle para avisos
Tab:AddToggle({
Name = "Avisos",
Default = false,
Callback = function(state)
avisoToggle = state
end
})

local Section = Tab:AddSection({
Name = "MATAR JOGADOR"
})
local selectedKillAdvancedPlayer = nil
local couchEquipped = false

local function killAdvancedPlayer()
if selectedKillAdvancedPlayer then
local player = game.Players:FindFirstChild(selectedKillAdvancedPlayer)
if player then
-- Equipa o item 'Couch' no inventÃ¡rio se ainda nÃ£o estiver equipado
local backpack = game.Players.LocalPlayer.Backpack
if backpack and not couchEquipped then
local couch = backpack:FindFirstChild("Couch")
if couch then
game.Players.LocalPlayer.Character.Humanoid:EquipTool(couch)
couchEquipped = true
else
print("O item 'Couch' nÃ£o foi encontrado no seu inventÃ¡rio.")
end
end

-- Looping de teleportes no jogador selecionado da lista
while true do
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = player.Character.HumanoidRootPart.CFrame
wait(0.0) -- Intervalo entre cada teleporte, ajuste conforme necessÃ¡rio

-- Verifica se o jogador sentou no 'Couch' e realiza o teleporte para o cÃ©u
if player.Character:FindFirstChild("Humanoid") and player.Character.Humanoid.SeatPart then
player.Character.HumanoidRootPart.CFrame = CFrame.new(0, 0, 0) -- Teleporta para cima
wait(0.0) -- Espera um pouco antes de teleportar de volta para evitar bugs
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(0, 0, 0) -- Teleporta para cima novamente
wait(0.0) -- Espera um pouco antes de teleportar de volta para evitar bugs
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(649.78, -439.87, 126.35) -- Teleporta de volta para a posiÃ§Ã£o original
break -- Sai do loop apÃ³s teleportar de volta
end
end

-- Remove o item 'Couch' da mÃ£o do jogador apÃ³s o teleporte para o cÃ©u
if couchEquipped then
local backpack = game.Players.LocalPlayer.Backpack
if backpack then
local couch = backpack:FindFirstChild("Couch")
if couch then
couch.Parent = nil -- Remove o 'Couch' do inventÃ¡rio
couchEquipped = false
end
end
end
else
print("Jogador nÃ£o encontrado.")
end
else
print("Nenhum jogador selecionado para o Bring AvanÃ§ado.")
end
end

-- Lista de Players para Bring AvanÃ§ado
local killAdvancedPlayerList = {}
for _, player in ipairs(game.Players:GetPlayers()) do
table.insert(killAdvancedPlayerList, player.Name)
end

Tab:AddDropdown({
Name = "Selecionar Jogador",
Description = "Selecione o jogador alvo para o Bring AvanÃ§ado",
Options = killAdvancedPlayerList,
Callback = function(playerName)
selectedKillAdvancedPlayer = playerName
end
})

Tab:AddButton({
Name = "KILL (ative A animacÃ£o Deite-se)",
Description = "Equipa o item 'Couch' e teleporta o jogador selecionado",
Callback = function()
killAdvancedPlayer()
end
})

Tab:AddButton({
Name = "Puxar Todos (Trollar)",
     Callback = function()
     --KK
     
     local args = {
    [1] = "ClearAllTools"
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Clea1rTool1s"):FireServer(unpack(args))

	--Couch Item
	
	local args = {
    [1] = "PickingTools",
    [2] = "Couch"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))

--Fling aall

loadstring(game:HttpGet("https://pastebin.com/raw/zqyDSUWX"))()
   end
})

local Section = Tab:AddSection({
Name = "Kill V2 [!] PODE CONTER ERROS"
})


-- ServiÃ§os necessÃ¡rios
local playerService = game:GetService('Players')
local runService = game:GetService('RunService')
local localPlayer = playerService.LocalPlayer
local backpack = localPlayer:FindFirstChildOfClass('Backpack')

-- VariÃ¡veis globais
local flingV14Toggle = false
local selectedFlingPlayerV14 = nil
local flingV14Connection
local playerSpawnedConnection
local spinFlingConnection
local bodyThrust

-- FunÃ§Ã£o para obter a lista de jogadores
local function getPlayerList()
    local playerList = {}
    for _, player in ipairs(playerService:GetPlayers()) do
        if player ~= localPlayer then
            table.insert(playerList, player.Name)
        end
    end
    return playerList
end

-- FunÃ§Ã£o para atualizar o dropdown
local function updateDropdown(dropdown)
    UpdateDropdown(dropdown, getPlayerList())
end

-- FunÃ§Ã£o para equipar o item "Couch"
local function equipCouch()
    print("Tentando equipar o sofÃ¡...")
    if backpack then
        local couchItem = backpack:FindFirstChild("Couch")
        if couchItem then
            couchItem.Parent = localPlayer.Character
            local humanoid = localPlayer.Character:FindFirstChildOfClass("Humanoid")
            if humanoid then
                humanoid:EquipTool(couchItem)
                print("SofÃ¡ equipado.")
            else
                print("Humanoide nÃ£o encontrado.")
            end
        else
            warn("SofÃ¡ nÃ£o encontrado na mochila.")
        end
    else
        print("Mochila nÃ£o encontrada.")
    end
end

-- FunÃ§Ã£o para teleportar para a coordenada
local function teleportToCoordinate()
    print("Teleportando para a coordenada...")
    local teleportPosition = Vector3.new(-499.00, -783.95, 239.72) -- Coordenada para onde vocÃª deseja teleportar
    localPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(teleportPosition)
end

-- FunÃ§Ã£o para flingar jogador (V14)
local function flingV14(targetPlayerName)
    local targetPlayer = playerService:FindFirstChild(targetPlayerName)
    if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
        -- Looping de teleportes no jogador selecionado
        flingV14Connection = runService.Heartbeat:Connect(function()
            if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                local targetPosition = targetPlayer.Character.HumanoidRootPart.CFrame
                      if toggle then
                    -- Teleportando uma unidade para cima
                    localPlayer.Character.HumanoidRootPart.CFrame = targetPosition * CFrame.new(0, 2, 0)
                else
                    -- Teleportando uma unidade para baixo
                    localPlayer.Character.HumanoidRootPart.CFrame = targetPosition * CFrame.new(0, -1, 0)
                end
                toggle = not toggle
            end

            -- Verifica se o jogador sentou no 'SeatCouch' e realiza o teleporte para a coordenada
            if targetPlayer.Character:FindFirstChild("Humanoid") and targetPlayer.Character.Humanoid.SeatPart then
                teleportToCoordinate()
                flingV14Connection:Disconnect()
                flingV14Connection = nil
            end
        end)
    else
        print("Jogador alvo nÃ£o encontrado ou invÃ¡lido.")
    end
end

-- FunÃ§Ã£o para detectar quando o jogador renasce
local function onPlayerRespawned(targetPlayer)
    if flingV14Toggle then
        -- Aguardar o renascimento do jogador
        playerSpawnedConnection = targetPlayer.CharacterAdded:Connect(function()
            wait(0.0) -- Tempo para garantir que o personagem foi totalmente carregado
            if flingV14Toggle then
                equipCouch() -- Equipar o sofÃ¡ quando o jogador renasce
                flingV14(targetPlayer.Name)
            end
        end)
    end
end

-- FunÃ§Ã£o para verificar e equipar o sofÃ¡ caso nÃ£o esteja equipado
local function ensureCouchEquipped()
    local character = localPlayer.Character
    if character then
        local equipped = false
        for _, tool in ipairs(character:GetChildren()) do
            if tool:IsA("Tool") and tool.Name == "Couch" then
                equipped = true
                break
            end
        end
        if not equipped then
            equipCouch()
        end
    else
        print("Personagem nÃ£o encontrado.")
    end
end

-- FunÃ§Ã£o para desativar as colisÃµes do personagem
local function disableCollisions(character)
    if character and character:FindFirstChild("Head") and character:FindFirstChild("UpperTorso") and character:FindFirstChild("LowerTorso") and character:FindFirstChild("HumanoidRootPart") then
        character.Head.CanCollide = false
        character.UpperTorso.CanCollide = false
        character.LowerTorso.CanCollide = false
        character.HumanoidRootPart.CanCollide = false
    else
        print("Parte do personagem nÃ£o encontrada.")
    end
end

-- FunÃ§Ã£o para reativar as colisÃµes do personagem
local function resetCollisions(character)
    if character and character:FindFirstChild("Head") and character:FindFirstChild("UpperTorso") and character:FindFirstChild("LowerTorso") and character:FindFirstChild("HumanoidRootPart") then
        character.Head.CanCollide = true
        character.UpperTorso.CanCollide = true
        character.LowerTorso.CanCollide = true
        character.HumanoidRootPart.CanCollide = true
    end
end

-- FunÃ§Ã£o para ativar o Spin Fling
local function activateSpinFling()
    local character = localPlayer.Character or localPlayer.CharacterAdded:Wait()
    local power = 1500 -- Pode ajustar isso para alterar a forÃ§a do impulso

    -- Cria um novo BodyThrust
    bodyThrust = Instance.new("BodyThrust")
    bodyThrust.Parent = character.HumanoidRootPart
    bodyThrust.Force = Vector3.new(power, 1100, power)
    bodyThrust.Location = character.HumanoidRootPart.Position

    -- Desativa as colisÃµes
    spinFlingConnection = runService.Stepped:Connect(function()
        disableCollisions(character)
    end)

    print("Spin Fling ativado!")
end

-- FunÃ§Ã£o para desativar o Spin Fling
local function deactivateSpinFling()
    local character = localPlayer.Character or localPlayer.CharacterAdded:Wait()

    -- Desconecta a funÃ§Ã£o de desativaÃ§Ã£o de colisÃµes
    if spinFlingConnection then
        spinFlingConnection:Disconnect()
        spinFlingConnection = nil
    end

    -- Remove o efeito de impulso do corpo
    if bodyThrust then
        bodyThrust:Destroy()
        bodyThrust = nil
    end

    -- Restaura as colisÃµes do personagem
    resetCollisions(character)

    print("Spin Fling desativado!")
end

-- FunÃ§Ã£o de callback para o toggle
local function onFlingV14Toggle(value)
    print("Toggle Fling V14:", value)
    flingV14Toggle = value
    if flingV14Toggle and selectedFlingPlayerV14 then
        local targetPlayer = playerService:FindFirstChild(selectedFlingPlayerV14)
        if targetPlayer then
            ensureCouchEquipped() -- Equipar o sofÃ¡ ao ativar o toggle
            flingV14(targetPlayer.Name)
            onPlayerRespawned(targetPlayer)
            activateSpinFling() -- Ativar o Spin Fling quando o toggle Ã© ativado
        end
    elseif not flingV14Toggle then
        -- Desconecta as conexÃµes quando o toggle Ã© desativado
        if flingV14Connection then
            flingV14Connection:Disconnect()
            flingV14Connection = nil
        end
        if playerSpawnedConnection then
            playerSpawnedConnection:Disconnect()
            playerSpawnedConnection = nil
        end
        deactivateSpinFling() -- Desativar o Spin Fling quando o toggle Ã© desativado
    end
end

-- Dropdown para selecionar o jogador
Tab:AddDropdown({
    Name = "Selecione o Jogador para Fling",
    Options = getPlayerList(),
    Default = "",
    Callback = function(playerName)
        selectedFlingPlayerV14 = playerName
        print("Jogador selecionado para Fling:", playerName)
    end
})

-- Atualiza a lista de jogadores quando jogadores entram ou saem do jogo
playerService.PlayerAdded:Connect(function()
    print("Novo jogador adicionado. Atualizando a lista...")
    updateDropdown(PlayerDropdown)
end)

playerService.PlayerRemoving:Connect(function()
    print("Jogador saiu. Atualizando a lista...")
    updateDropdown(PlayerDropdown)
end)

-- Toggle para ativar/desativar o Fling
Tab:AddToggle({
    Name = "Matar Jogador Selecionado!",
    Default = false,
    Callback = function(value)
        print("Toggle Fling V14:", value)
        flingV14Toggle = value
        if flingV14Toggle and selectedFlingPlayerV14 then
            local targetPlayer = playerService:FindFirstChild(selectedFlingPlayerV14)
            if targetPlayer then
                ensureCouchEquipped() -- Equipar o sofÃ¡ ao ativar o toggle
                flingV14(targetPlayer.Name)
                onPlayerRespawned(targetPlayer)
                activateSpinFling() -- Ativar o Spin Fling quando o toggle Ã© ativado
            end
        elseif not flingV14Toggle then
            -- Desconecta as conexÃµes quando o toggle Ã© desativado
            if flingV14Connection then
                flingV14Connection:Disconnect()
                flingV14Connection = nil
            end
            if playerSpawnedConnection then
                playerSpawnedConnection:Disconnect()
                playerSpawnedConnection = nil
            end
            deactivateSpinFling() -- Desativar o Spin Fling quando o toggle Ã© desativado
        end
    end
})

local avTab = Window:MakeTab({
Name = "Avatar | Brookhaven",
Icon = "rbxassetid://414904019",
PremiumOnly = false
})

avTab:AddToggle({
Name = "Mudar de cor Automaticamente",
Default = false,
Callback = function(value)
isActive = value
while isActive do
local colors = {"Eggplant", "Medium stone grey", "Dark nougat", "Really red", "New Yeller", "Lime green", "Toothpaste", "Dirt brown"}
for _, color in ipairs(colors) do
local args = {
[1] = "skintone",
[2] = color
}
game:GetService("ReplicatedStorage").RE:FindFirstChild("1Updat1eAvata1r"):FireServer(unpack(args))
wait(1) -- espera 1 segundo entre as execuÃ§Ãµes
end
end
end
})

avTab:AddButton({
Name = "Virar PÃ³ Brilhante",
     Callback = function()
     --Size
     
     local args = {
    [1] = "CharacterSizeDown",
    [2] = 4
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Clothe1s"):FireServer(unpack(args))

--Item

local args = {
    [1] = "wear",
    [2] = 173624651
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Updat1eAvata1r"):FireServer(unpack(args))

--outro

local args = {
    [1] = "wear",
    [2] = 141742418
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Updat1eAvata1r"):FireServer(unpack(args))
end
})
local Section = avTab:AddSection({
	Name = "Extras"
})
avTab:AddButton({
Name = "Virar Assasino",
	Callback = function()
	--Assain

	local args = {
    [1] = "wear",
    [2] = 15133320948
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Updat1eAvata1r"):FireServer(unpack(args))
end
})
avTab:AddButton({
Name = "Babiromet do davi scripts",
	Callback = function()
	--Assain

	local args = {
    [1] = "CharacterChange",
    [2] = {
        [1] = 14731377941,
        [2] = 14731377894,
        [3] = 14731377875,
        [4] = 14731384498,
        [5] = 14731377938,
        [6] = 0
    },
    [3] = "Stick Bug"
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Avata1rOrigina1l"):FireServer(unpack(args))

--Man

local args = {
    [1] = "wear",
    [2] = 6564572490
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Updat1eAvata1r"):FireServer(unpack(args))

--speed

game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 90
end
})
avTab:AddButton({
Name = "Virar Esqueleto (F.E)",
     Callback = function()
     local args = {
    [1] = "CharacterChange",
    [2] = {
        [1] = 36781360,
        [2] = 36781407,
        [3] = 36781447,
        [4] = 36781481,
        [5] = 36781518,
        [6] = 0
    },
    [3] = "Skeleton"
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Avata1rOrigina1l"):FireServer(unpack(args))
end
})

local gtTab = Window:MakeTab({
Name = "Itens | Brookhaven",
Icon = "rbxassetid://414904019",
PremiumOnly = false
})

local function AddButton(name, tool)
    gtTab:AddButton({
        Name = name,
        Callback = function()
            local args = {
                [1] = "PickingTools",
                [2] = tool
            }
            local remoteFunction = game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l")
            if remoteFunction the


local redzlib =       
        
        
        loadstring(game: HttpGet("https://raw.githubusercontent.com/YimGg992/Segaz-HubZ/refs/heads/main/README.md"))()    
