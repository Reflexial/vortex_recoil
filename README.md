# vortex_recoil
Crappy recoil reduction AutoHotKey script

Get AutoHotkey to use the script, or use handy compiled .exe.


Press F3 to turn on/off.
When you left click, it will adjust your mouse downward based on the compensation value set.
Press numpad +/- and you should see a tooltip pop up showing you what value your adjustment amount is at.  Probably needs to be ~5 to have a real effect.

Run as administrator.

Here is the script code:









;#####################
;#     Commands     #
;#####################
SendMode Input 
SetWorkingDir %A_ScriptDir% 
#NoEnv
#InstallKeybdHook
#InstallMouseHook
SetDefaultMouseSpeed, 0
SetMouseDelay, -1
SetKeyDelay, -1
SetWinDelay, -1
SetBatchLines, -1
SetControlDelay -1
#MaxThreads 30
#MaxThreadsBuffer on
#KeyHistory 0
SendMode Input
#UseHook
 
;#####################
;#     Variables     #
;#####################
_nr := False
Compensation = 1
_compVal = 1 ; Compensation value
 
~LButton::norecoil()  ; Left Trigger
~F3:: ; Turn script on/off
 
_nr := ! _nr ; dont touch
return
 
norecoil()
{
    global 
    if _nr
    {
        Loop{
            if Compensation = 1 
            if GetKeyState("LButton", "P")
            {
                mouseXY(0, _compVal)
                Sleep 30
                mouseXY(0, _compVal)
                Sleep 50
            }
            else
            break
        } ;; loop
    } ;; if
} ;; norecoil()
 
 
 
;########################
;#     Compensation     #
;########################
 
mouseXY(x,y)
{
  DllCall("mouse_event",uint,1,int,x,int,y,uint,0,int,0)
}
 
 
;###################
;#     ToolTip     #
;###################
 
ToolTip(Text)
{
 
  ToolTip, %Text%, xPos, yPos
  SetTimer, RemoveToolTip, 1300
  return
  RemoveToolTip:
  SetTimer, RemoveToolTip, Off
  ToolTip
  Return
}
 
~NumpadAdd:: ; Adds compensation.
  _compVal := _compVal + 0.10
  ToolTip("Compensation " . _compVal)
Return
 
~NumpadSub:: ; Substracts compensation.
if _compVal > 0
{
  _compVal := _compVal - 0.10
  ToolTip("Compensation " . _compVal)
 }
return
;###################
;#  AimCorrection  #
;###################
 
~*$LButton::
Loop
{
GetKeyState, state, Lbutton, P
if state=u
break
mouseXY(1,1)
sleep 1
mouseXY(Slider,1)
sleep 1
mouseXY(Slider,-1)
sleep 1
mouseXY(-1,0)
sleep 1
mouseXY(Slider,1)
sleep 1
mouseXY(Slider,-1)
sleep 1
}
return
mouseXY(x,y)
