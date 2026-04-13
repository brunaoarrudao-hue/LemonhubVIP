-- [[ SERVIÇOS ]] --
local player = game.Players.LocalPlayer
local VirtualUser = game:GetService("VirtualUser")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- [[ VARIÁVEIS GLOBAIS ]] --
_G.FastAttack = true
_G.AttackSpeed = 0.05 -- Podes baixar para 0 se quiseres a velocidade máxima do ficheiro original

-- [[ FUNÇÃO DE ATAQUE (IGUAL AO TEU ARQUIVO TSUO.TXT) ]] --
task.spawn(function()
    while task.wait(_G.AttackSpeed) do
        if _G.FastAttack then
            pcall(function()
                local character = player.Character
                local tool = character and character:FindFirstChildOfClass("Tool")
                
                if tool then
                    -- 1. Clique Físico (Coordenadas exatas do tsuo.txt)
                    VirtualUser:CaptureController()
                    VirtualUser:ClickButton1(Vector2.new(851, 158), workspace.CurrentCamera.CFrame)
                    
                    -- 2. Ativação da Arma
                    tool:Activate()

                    -- 3. Lógica do CombatFramework (Retirada do tsuo.txt)
                    local CombatFramework = require(player.PlayerScripts.CombatFramework)
                    local CombatFrameworkLib = require(player.PlayerScripts.CombatFramework.CombatFrameworkLib)
                    
                    if CombatFramework.activeController and CombatFramework.activeController.equippedWeapon then
                        local controller = CombatFramework.activeController
                        
                        -- Remove animação e aplica o hit rápido
                        CombatFrameworkLib.ShowMeleeEffect(controller.equippedWeapon)
                        controller:attack()
                    end
                end
            end)
        end
    end
end)

-- [[ INTERFACE RAYFIELD (LEMON HUB) ]] --
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "🍋 Lemon Hub v4.9",
   LoadingTitle = "A carregar Script...",
   LoadingSubtitle = "by tsuo Edit",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "LemonHubConfig",
      FileName = "Main"
   }
})

local Tab1 = Window:CreateTab("Principal", "bolt")

Tab1:CreateSection("Sistemas de Combate")

Tab1:CreateToggle({
   Name = "Auto Click / Fast Attack",
   CurrentValue = _G.FastAttack,
   Flag = "FastAttackToggle", 
   Callback = function(Value)
      _G.FastAttack = Value
   end,
})

Tab1:CreateSlider({
   Name = "Velocidade de Ataque",
   Min = 0,
   Max = 1,
   CurrentValue = 0.05,
   Flag = "AttackSpeedSlider",
   Callback = function(Value)
      _G.AttackSpeed = Value
   end,
})

-- IMPORTANTE: O Rayfield precisa disto para o script aparecer!
Rayfield:LoadConfiguration()
