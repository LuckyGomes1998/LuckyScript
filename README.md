-- Armazena os IDs dos lugares para fácil referência
local WORLDS = {
    [2753915549] = "World1",
    [4442272183] = "World2",
    [7449423635] = "World3"
}

-- Tabela centralizada para todas as informações das missões
local QUEST_DATA = {
    -- #################### MUNDO 1 ####################
    {
        World = "World1", MinLevel = 1, MaxLevel = 9, Mon = "Bandit", LevelQuest = 1, NameQuest = "BanditQuest1",
        CFrameQuest = CFrame.new(1059.37, 15.45, 1550.42, 0.94, 0, -0.34, 0, 1, 0, 0.34, 0, 0.94),
        CFrameMon = CFrame.new(1045.96, 27.00, 1560.82)
    },
    {
        World = "World1", MinLevel = 10, MaxLevel = 14, Mon = "Monkey", LevelQuest = 1, NameQuest = "JungleQuest",
        CFrameQuest = CFrame.new(-1598.09, 35.55, 153.38, 0, 0, 1, 0, 1, 0, -1, 0, 0),
        CFrameMon = CFrame.new(-1448.52, 67.85, 11.47)
    },
    {
        World = "World1", MinLevel = 15, MaxLevel = 29, Mon = "Gorilla", LevelQuest = 2, NameQuest = "JungleQuest",
        CFrameQuest = CFrame.new(-1598.09, 35.55, 153.38, 0, 0, 1, 0, 1, 0, -1, 0, 0),
        CFrameMon = CFrame.new(-1129.88, 40.46, -525.42)
    },
    -- ... Adicione todas as outras missões dos mundos 1, 2 e 3 aqui no mesmo formato ...
    {
        World = "World3", MinLevel = 2525, MaxLevel = math.huge, Mon = "Isle Champion", LevelQuest = 2, NameQuest = "TikiQuest2",
        CFrameQuest = CFrame.new(-16539.08, 55.69, 1051.57),
        CFrameMon = CFrame.new(-16933.21, 93.35, 999.45)
    }
}

-- Serviços do Jogo
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- Variáveis de estado do jogo
local currentWorld = WORLDS[game.PlaceId]
if not currentWorld then
    LocalPlayer:Kick("Este mundo não é suportado.")
end

-- Função otimizada para obter os dados da missão atual
function CheckQuest()
    local myLevel = LocalPlayer.Data.Level.Value

    for _, quest in ipairs(QUEST_DATA) do
        if quest.World == currentWorld and myLevel >= quest.MinLevel and myLevel <= quest.MaxLevel then
            -- Atribui todas as variáveis de uma vez
            Mon = quest.Mon
            LevelQuest = quest.LevelQuest
            NameQuest = quest.NameQuest
            NameMon = quest.Mon -- NameMon é frequentemente o mesmo que Mon
            CFrameQuest = quest.CFrameQuest
            CFrameMon = quest.CFrameMon
            
            -- Lógica específica de teleporte (se necessário)
            if (quest.Mon == "Fishman Warrior" or quest.Mon == "Fishman Commando") and _G.AutoFarm then
                 if (quest.CFrameQuest.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude > 10000 then
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("requestEntrance", Vector3.new(61163.85, 11.68, 1819.78))
                 end
            end
            -- Adicione outras lógicas de teleporte aqui...

            return -- Encontrou a missão, então pode sair da função
        end
    end
end
