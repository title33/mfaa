local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/title33/SaveManager/main/README.md"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "Xylo Hub",
    SubTitle = "by Sky",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true, -- The blur may be detectable, setting this to false disables blur entirely
    Theme = "Amethyst",
    MinimizeKey = Enum.KeyCode.LeftControl -- Used when theres no MinimizeKeybind
})

--Fluent provides Lucide Icons https://lucide.dev/icons/ for the tabs, icons are optional
local Tabs = {
    General = Window:AddTab({ Title = "General", Icon = "monitor" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}

local Options = Fluent.Options



    local Toggle = Tabs.General:AddToggle("MyToggle", {Title = "Auto Farm", Default = false })

    Toggle:OnChanged(function(A)
        _G.AutoFarm = A

spawn(function()
while wait(.1) do
    pcall(function()
if _G.AutoFarm then
game:GetService("ReplicatedStorage").Events.Fight.ClickDamage:FireServer()

                end
        end)
   end
end)

    end)

    Options.MyToggle:SetValue(false)



    local Toggle = Tabs.General:AddToggle("MyToggle", {Title = "Auto RebirthUp", Default = false })

    Toggle:OnChanged(function(A)
                _G.RebirthUp = A

spawn(function()
while wait(.1) do
    pcall(function()
if _G.RebirthUp then
game:GetService("ReplicatedStorage").Events.Stats.RebirthUp:FireServer()


                end
        end)
   end
end)

    end)
    Options.MyToggle:SetValue(false)

    local Slider = Tabs.General:AddSlider("Slider", {
        Title = "Walk speed",
        Description = "speed slider",
        Default = 0,
        Min = 0,
        Max = 300,
        Rounding = 1,
        Callback = function(Value)
            game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = (Value)
        end
    })

    Slider:OnChanged(function(Value)
       game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = (Value)
    
    end)

    Slider:SetValue(15)


    Tabs.General:AddButton({
        Title = "Fly",
        Description = "Fly",
        Callback = function()
            Window:Dialog({
                Title = "Title",
                Content = "This is a dialog",
                Buttons = {
                    {
                        Title = "Confirm",
                        Callback = function()
                            loadstring(game:HttpGet('https://pastebin.com/raw/YSL3xKYU'))()
                        end
                    },
                    {
                        Title = "Cancel",
                        Callback = function()
                            print("Cancelled the dialog.")
                        end
                    }
                }
            })
        end
    })





 


-- Addons:
-- SaveManager (Allows you to have a configuration system)
-- InterfaceManager (Allows you to have a interface managment system)

-- Hand the library over to our managers
SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)

-- Ignore keys that are used by ThemeManager.
-- (we dont want configs to save themes, do we?)
SaveManager:IgnoreThemeSettings()

-- You can add indexes of elements the save manager should ignore
SaveManager:SetIgnoreIndexes({})

-- use case for doing it this way:
-- a script hub could have themes in a global folder
-- and game configs in a separate folder per game
InterfaceManager:SetFolder("FluentScriptHub")
SaveManager:SetFolder("FluentScriptHub/specific-game")

InterfaceManager:BuildInterfaceSection(Tabs.Settings)
SaveManager:BuildConfigSection(Tabs.Settings)


Window:SelectTab(1)


-- You can use the SaveManager:LoadAutoloadConfig() to load a config
-- which has been marked to be one that auto loads!
SaveManager:LoadAutoloadConfig()
