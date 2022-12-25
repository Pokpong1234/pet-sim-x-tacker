function comma_value(amount)
    local formatted = amount
    while true do  
      formatted, k = string.gsub(formatted, "^(-?%d+)(%d%d%d)", '%1,%2')
      if (k==0) then
        break
      end
    end
    return formatted
  end
  
  ---============================================================
  -- rounds a number to the nearest decimal places
  --
  function round(val, decimal)
    if (decimal) then
      return math.floor( (val * 10^decimal) + 0.5) / (10^decimal)
    else
      return math.floor(val+0.5)
    end
  end
  
  --===================================================================
  -- given a numeric value formats output with comma to separate thousands
  -- and rounded to given decimal places
  --
  --
  function format_num(amount, decimal, prefix, neg_prefix)
    local str_amount,  formatted, famount, remain
  
    decimal = decimal or 2  -- default 2 decimal places
    neg_prefix = neg_prefix or "-" -- default negative sign
  
    famount = math.abs(round(amount,decimal))
    famount = math.floor(famount)
  
    remain = round(math.abs(amount) - famount, decimal)
  
          -- comma to separate the thousands
    formatted = comma_value(famount)
  
          -- attach the decimal portion
    if (decimal > 0) then
      remain = string.sub(tostring(remain),3)
      formatted = formatted .. "." .. remain ..
                  string.rep("0", decimal - string.len(remain))
    end
  
          -- attach prefix string e.g '$' 
    formatted = (prefix or "") .. formatted 
  
          -- if value is negative then format accordingly
    if (amount<0) then
      if (neg_prefix=="()") then
        formatted = "("..formatted ..")"
      else
        formatted = neg_prefix .. formatted 
      end
    end
  
    return formatted
  end
--==================================================================


-- don't mess with anything here may cause script to not send a webhook!
  _G.trackD = 'Diamonds'
_G.trackC = 'Coins'
_G.Tracking = 'Fantasy Coins' 
_G.rconsole = false --keep this false!!!!!!!!! (script would still work on true but it's a thing for synapse users and it's not needed)
_G.TableThing = {}
_G.TableThingd = {}
_G.TableThingc = {}
_G.stop = false -- keep this false or webhook wont send!!!


while not _G.stop do
local Player = Players.LocalPlayer
local d  = string.gsub(game.Players.LocalPlayer.PlayerGui.Main.Right.Diamonds.Amount.Text, ",", "")
local r  = string.gsub(game.Players.LocalPlayer.PlayerGui.Main.Right.Rank.RankName.Text, ",", "")
local c  = string.gsub(game.Players.LocalPlayer.PlayerGui.Main.Right.Coins.Amount.Text, ",", "")
local f  = string.gsub(game.Players.LocalPlayer.PlayerGui.Main.Right[_G.Tracking].Amount.Text, ",", "")
local GoldHaunted = f / 31500000
local Haunted = f / 3500000
local Enchant = d / 10000
    local startval = string.gsub(game.Players.LocalPlayer.PlayerGui.Main.Right[_G.Tracking].Amount.Text, ",", "")
    local startvald = string.gsub(game.Players.LocalPlayer.PlayerGui.Main.Right[_G.trackD].Amount.Text, ",", "")
    local startvalc = string.gsub(game.Players.LocalPlayer.PlayerGui.Main.Right[_G.trackC].Amount.Text, ",", "")
    if _G.rconsole then
        rconsoleprint('@@LIGHT_MAGENTA@@')
        rconsoleprint("Starting with: " .. startval .. "\n")
    else
        print("Sending weebhook in 60 secs....")
    end

    wait(60)

    local endval = string.gsub(game.Players.LocalPlayer.PlayerGui.Main.Right[_G.Tracking].Amount.Text, ",", "")
    local endvald = string.gsub(game.Players.LocalPlayer.PlayerGui.Main.Right[_G.trackD].Amount.Text, ",", "")
    local endvalc = string.gsub(game.Players.LocalPlayer.PlayerGui.Main.Right[_G.trackC].Amount.Text, ",", "")
    local diffy = tonumber(endval) - tonumber(startval)
    local diffyd = tonumber(endvald) - tonumber(startvald)
    local diffyc = tonumber(endvalc) - tonumber(startvalc)
    if _G.rconsole then
        rconsoleprint('@@LIGHT_CYAN@@')
        rconsoleprint("Ended with: " .. endval .. " || Gained: " .. diffy .. " in 60 seconds \n")
    else
        print("WebHook Sent! If no webhook was sent you did something wrong. Script made by foro#8122")
    end
    table.insert(_G.TableThing, diffy)
    table.insert(_G.TableThingd, diffyd)
    table.insert(_G.TableThingc, diffyc)
    
    local b = 0
    for i,v in pairs(_G.TableThing) do
        b = b + v
    end
    local bc = 0
    for i,v in pairs(_G.TableThingc) do
        bc = b + v
    end
    local bd = 0
    for i,v in pairs(_G.TableThingd) do
        bd = b + v
    end
    if _G.rconsole then
        rconsoleprint('@@GREEN@@')
        rconsoleprint("Total: " .. b .. " in " .. #_G.TableThing .. " mins || average per min:" .. b/#_G.TableThing .. "\n \n")
    else
        local webhookcheck =
   is_sirhurt_closure and "s" or pebc_execute and "p" or syn and "s" or
   secure_load and "s" or
   KRNL_LOADED and "k" or
   SONA_LOADED and "s" or
   "e"

local url = YourWebHookHere
   
local data = {
   ["content"] = "",
		["embeds"] = {{
			["title"] = "__**Pet Simulator Stat Tracker**__",
			["description"] = "made by foro#8122", -- Don't change? 
			["type"] = "rich",
			["color"] = tonumber(0x0E980E),
			["fields"] = {
        {
					["name"] = "__Rank__",
					["value"] = r,
					["inline"] = false
				},
				{
					["name"] = "__Coins__",
					["value"] = comma_value(c),
					["inline"] = false
				},
				{
					["name"] = "__Fantasy Coins__",
					["value"] = comma_value(f),
					["inline"] = false
				},
				{
					["name"] = "__Diamonds__",
					["value"] = comma_value(d),
					["inline"] = false
				},
                {
					["name"] = "__Golden Haunted Egg__",
					["value"] = (comma_value(round(GoldHaunted))),
					["inline"] = false
				},
				{
					["name"] = "__Normal Haunted Egg__",
					["value"] = (comma_value(round(Haunted))),
					["inline"] = false
				},
				{
					["name"] = "__Enchant Purchases__",
					["value"] = (comma_value(round(Enchant))),
					["inline"] = false
				},
                {
					["name"] = "__Coins Earned In The Last Minute__",
					["value"] = "Gained: " .. comma_value(diffyc) .. " In 60 Seconds",
					["inline"] = false
				},
                {
					["name"] = "__Diamonds Earned In The Last Minute__",
					["value"] = "Gained: " .. comma_value(diffyd) .. " In 60 Seconds",
					["inline"] = false
				},
                {
					["name"] = "__Fantasy Coins Earned In The Last Minute__",
					["value"] = "Gained: " .. comma_value(diffy) .. " In 60 Seconds",
					["inline"] = false
				},
                {
					["name"] = "__Fantasy Coins Earned During This Session__",
					["value"] = "Total: " .. comma_value(b) .. " in " .. #_G.TableThing .. " mins || average per min: " .. (comma_value(round(b/#_G.TableThing))),
					["inline"] = false
				},
			}
		}}
	}
local newdata = game:GetService("HttpService"):JSONEncode(data)

local headers = {
   ["content-type"] = "application/json"
}
request = http_request or request or HttpPost or syn.request
local abcdef = {Url = url, Body = newdata, Method = "POST", Headers = headers}
request(abcdef)
    end
end
