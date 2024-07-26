-- Fonction pour infliger des dégâts au joueur
local function applyDamage(player)
    -- Vérifie si le joueur a un Humanoid
    local humanoid = player:FindFirstChildOfClass("Humanoid")
    if humanoid then
        -- Inflige 3 de dégâts
        humanoid:TakeDamage(3)
    end
end

-- Fonction pour appliquer l'effet de virus au joueur
local function applyVirusEffect(player)
    -- Change la couleur du joueur en vert
    local character = player.Character
    if character then
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                part.BrickColor = BrickColor.new("Bright green")
            end
        end
    end

    -- Inflige des dégâts chaque seconde
    while wait(1) do
        applyDamage(player)
    end
end

-- Fonction pour vérifier les collisions entre joueurs
local function onPlayerTouch(otherPlayer)
    -- Vérifie si l'autre joueur est un joueur valide
    if otherPlayer and otherPlayer:IsA("Player") then
        -- Applique l'effet de virus à l'autre joueur
        applyVirusEffect(otherPlayer)
    end
end

-- Fonction pour gérer le contact avec le bloc
local function onTouch(hit)
    -- Vérifie si l'objet touché est un joueur
    local player = game.Players:GetPlayerFromCharacter(hit.Parent)
    if player then
        -- Applique l'effet de virus au joueur
        applyVirusEffect(player)

        -- Connecte l'événement de collision pour propager le virus
        hit.Touched:Connect(function(otherHit)
            local otherPlayer = game.Players:GetPlayerFromCharacter(otherHit.Parent)
            onPlayerTouch(otherPlayer)
        end)
    end
end

-- Connecte la fonction onTouch à l'événement Touched du bloc
script.Parent.Touched:Connect(onTouch)
