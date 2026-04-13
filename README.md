-- 1. LOAD RAYFIELD (Com link alternativo de backup)
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- 2. VARIÁVEIS (Exatamente como no Lemon Hub / tsuo.txt)
_G.FastAttack = true
_G.AttackSpeed = 0.05
local player = game.Players.LocalPlayer

-- 3. FUNÇÃO DE ATAQUE EXTRAÍDA DO TSUO.TXT
-- Colocamos um pcall forte para que o script não morra se o módulo não carregar
task.spawn(function()
    while true do
        task.wait(_G.AttackSpeed)
        if _G.FastAttack then
            pcall(function()
                local character = player.Character
                local tool = character and character:FindFirstChildOfClass("Tool")
                
                if tool and character:FindFirstChild("HumanoidRootPart") then
                    -- Clique visual do seu arquivo
                    game:GetService("VirtualUser"):CaptureController()
                    game:GetService("VirtualUser"):ClickButton1(Vector2.new(851, 158), workspace.CurrentCamera.CFrame)
                    
                    tool:Activate()

                    -- Parte do CombatFramework (O que faz bater rápido no seu arquivo)
                    local cf = require(player.PlayerScripts.CombatFramework)
                    local ac = cf.activeController
                    
                    if ac and ac.equippedWeapon then
                        -- Simula o hit sem delay
                        ac:attack()
                    end
                end
            end)
        end
    end
end)

-- 4. INTERFACE
local Window = Rayfield:CreateWindow({
   Name = "🍋 Lemon Hub v4.9",
   LoadingTitle = "Executando...",
   LoadingSubtitle = "by tsuo",
   ConfigurationSaving = { Enabled = false } -- Desativado para evitar erros de permissão de pasta
})

local Tab = Window:CreateTab("Principal", 4483345998)

Tab:CreateToggle({
   Name = "Fast Attack (tsuo style)",
   CurrentValue = true,
   Flag = "FA1",
   Callback = function(Value)
      _G.FastAttack = Value
   end,
})

-- Notificação para saber que o script rodou
Rayfield:Notify({
   Title = "Lemon Hub",
   Content = "Script carregado com sucesso!",
   Duration = 5
})
