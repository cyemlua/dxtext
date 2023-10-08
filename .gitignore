local myText = "DX'i GUI gibi kullanabilirsiniz."
w, h = dxGetTextSize(myText, 0, 5, "default-bold")
local x, y = (sx - w) / 2, (sy - h) / 2

selectedIndex = false
delays = {
    ["backspace"] = getTickCount(),
    ["arrow_l"] = getTickCount(),
    ["arrow_r"] = getTickCount(),
}
addEventHandler("onClientRender", root, function()
    dxDrawRectangle(0, 0, sx, sy, tocolor(31, 31, 31))
    if not selectedIndex then 
        dxDrawText(myText, x, y, x + w, y + h, tocolor(200, 200, 200), 5, "default-bold", "left", "center")
    else
        local first, second = myText:sub(1, selectedIndex), myText:sub(selectedIndex + 1)
        local firstWidth = dxGetTextWidth(first, 5, "default-bold")
        dxDrawText(myText, x, y, x + w, y + h, tocolor(200, 200, 200), 5, "default-bold", "left", "center")
        dxDrawLine((x + firstWidth) - 1, y, (x + firstWidth) - 1, (y + h) - 2, tocolor(200, 200, 200, 255), 2)
        if getKeyState("backspace") then
            if delays.backspace < getTickCount() and selectedIndex >= 1 then
                delays.backspace = getTickCount() + 100
                myText = first:sub(1, -2)..second
                selectedIndex = selectedIndex - 1
            end
        elseif getKeyState("arrow_l") then
            if delays.arrow_l < getTickCount() then
                delays.arrow_l = getTickCount() + 100
                selectedIndex = math.max(selectedIndex - 1, 0)
            end
        elseif getKeyState("arrow_r") then
            if delays.arrow_r < getTickCount() then
                delays.arrow_r = getTickCount() + 100
                selectedIndex = math.min(selectedIndex + 1, #myText)
            end
        end
    end
    if isMouseInPosition(x, y, w, h) then
        local mx, my = getMousePos(x, y)
        local char, index = getClosestCharacter(myText, mx)
        local myText = myText:sub(1, index - 1).."#ff0000"..char.."#ffffff"..myText:sub(index + 1)
        checkClick(index, char)
    else
        checkClick(false, false)
    end
end)

function addCharacter ( c )
    if selectedIndex then
        myText = myText:sub(1, selectedIndex)..c..myText:sub(selectedIndex + 1)
        selectedIndex = selectedIndex + 1
        w, h = dxGetTextSize(myText, 0, 5, "default-bold")
        for k, d in pairs(delays) do
            delays[k] = getTickCount() + 100
        end
    end
end
addEventHandler("onClientCharacter", root, addCharacter)

bindKey("home", "down", function()
    selectedIndex = 0
end)

bindKey("end", "down", function()
    selectedIndex = #myText
end)

function onTextClick ( i, c )
    selectedIndex = i
end

clicked = false
function checkClick (index, char)
    if getKeyState("mouse1") then
        if not clicked then
            clicked = true
        end
    else
        if clicked then
            clicked = false
            onTextClick(index, char)
        end
    end
end

function getClosestCharacter ( text, mx )
    if #text < 1 then text = " " end
    closestCharacter = false
    for i=1, #text do
        local startX = i ~= 1 and dxGetTextWidth(text:sub(1, i - 1), 5, "default-bold") or 0
        local diff = math.abs(mx - startX)
        if not closestCharacter then closestCharacter = {i, diff} elseif diff < closestCharacter[2] then closestCharacter = {i, diff} end
    end
    return text:sub(closestCharacter[1], closestCharacter[1]), closestCharacter[1]
end

function getMousePos ( startx, starty )
    local cx, cy = getCursorPosition()
    return (cx * sx) - startx, (cy * sy) - starty
end
showCursor(true)
