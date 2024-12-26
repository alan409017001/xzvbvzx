-- Script to prevent damage from mobs (NPCs) in Arceus (Roblox)

-- Assuming you have an NPC or mob with a custom damage function, we will override that to cancel the damage

local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- Function to cancel damage
local function onDamageTaken(player, damageSource, damageAmount)
    -- Check if the damage source is an NPC or mob
    if damageSource and damageSource:IsA("Model") and damageSource:FindFirstChild("Humanoid") then
        -- Check if the model is an NPC (you can modify the condition to suit your needs)
        local npc = damageSource
        if npc.Name == "Mob" then  -- Modify to the actual name of your NPC
            -- Prevent damage
            return 0  -- Return 0 to cancel the damage
        end
    end
    return damageAmount  -- Allow damage to continue for other sources
end

-- Connect the damage event to the function
ReplicatedStorage.DamageTaken.OnServerEvent:Connect(function(player, damageSource, damageAmount)
    local newDamageAmount = onDamageTaken(player, damageSource, damageAmount)
    -- Send the modified damage amount back to apply the change
    ReplicatedStorage.ApplyDamage:FireClient(player, newDamageAmount)
end)

