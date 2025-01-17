local player = game.Players.LocalPlayer
local camera = workspace.CurrentCamera
local mouse = player:GetMouse()

-- Função para criar a caixa ESP e a linha
local function createESP(target)
    -- Verifica se o alvo é um jogador
    if target:IsA("Model") and target:FindFirstChild("Humanoid") then
        -- Criar uma caixa ESP
        local box = Instance.new("Part")
        box.Anchored = true
        box.CanCollide = false
        box.Size = Vector3.new(4, 6, 4)  -- Tamanho da caixa
        box.Transparency = 0.5
        box.BrickColor = BrickColor.new("Bright red")
        box.Parent = workspace

        -- Criar a linha para o ESP
        local line = Drawing.new("Line")
        line.Thickness = 2
        line.Color = Color3.fromRGB(255, 0, 0)
        line.Visible = false  -- Inicialmente invisível
        line.Transparency = 1

        -- Atualiza a posição da caixa e a linha
        game:GetService("RunService").Heartbeat:Connect(function()
            if target and target:FindFirstChild("HumanoidRootPart") then
                -- Atualizando a posição da caixa
                box.Position = target.HumanoidRootPart.Position + Vector3.new(0, 3, 0)
                box.CFrame = CFrame.new(box.Position)
                
                -- Convertendo a posição do jogador para a tela
                local screenPos, onScreen = camera:WorldToScreenPoint(target.HumanoidRootPart.Position)
                
                -- Verifica se o jogador está visível ou se está atrás de uma parede
                if onScreen then
                    line.Visible = true
                    line.From = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2)  -- Posição da câmera
