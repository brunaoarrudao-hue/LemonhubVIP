-- [[ VARIÁVEIS DO LEMON HUB ]] --
_G.FastAttack = true
_G.AttackSpeed = 0.05
_G.BringMob = true

-- [[ SERVIÇOS ]] --
local player = game.Players.LocalPlayer
local VirtualUser = game:GetService("VirtualUser")

-- [[ FUNÇÃO DE ATAQUE IGUAL AO ARQUIVO TSUO.TXT ]] --
-- No teu arquivo, o ataque é disparado dentro de um loop spawn/task.spawn
task.spawn(function()
    while task.wait(_G.AttackSpeed) do
        if _G.FastAttack then
            pcall(function()
                local character = player.Character
                local tool = character and character:FindFirstChildOfClass("Tool")
                
                if tool then
                    -- 1. Simulação de Clique (Igual ao que está no teu arquivo)
                    VirtualUser:CaptureController()
                    VirtualUser:ClickButton1(Vector2.new(851, 158), game:GetService("Workspace").CurrentCamera.CFrame)
                    
                    -- 2. Ativação da Tool
                    tool:Activate()

                    -- 3. Implementação do CombatFramework (O segredo do Fast Attack do teu arquivo)
                    local CombatFramework = require(player.PlayerScripts.CombatFramework)
                    local CombatFrameworkLib = require(player.PlayerScripts.CombatFramework.CombatFrameworkLib)
                    
                    if CombatFramework.activeController and CombatFramework.activeController.equippedWeapon then
                        -- O teu script original usa esta sequência para bater muito rápido:
                        local controller = CombatFramework.activeController
                        
                        -- Remove a animação e regista o hit (Igual ao tsuo.txt)
                        CombatFrameworkLib.ShowMeleeEffect(controller.equippedWeapon)
                        controller:attack()
                    end
                end
            end)
        end
    end
end)

-- [[ INTERFACE RAYFIELD ]] --
local Window = Rayfield:CreateWindow({
   Name = "🍋 Lemon Hub v4.9",
   LoadingTitle = "Loading script",
   ConfigurationSaving = { Enabled = true, FolderName = "LemonHub" }
})

local Tab1 = Window:CreateTab("Principal", "bolt")

Tab1:CreateToggle({
   Name = "Auto Click / Fast Attack",
   CurrentValue = _G.FastAttack,
   Flag = "FastAttack",
   Callback = function(Value)
      _G.FastAttack = Value
   end,
})
