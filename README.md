mobIds={}

function shapeShift(p,m){
if(!m)return
if(!api.checkValid(m))return
unShapeShift(p)
api.addFollowingEntityToPlayer(p,m)
mobIds[p]=m
api.setTargetedPlayerSettingForEveryone(p,"opacity",0)
api.setTargetedPlayerSettingForEveryone(p,"nameTagInfo",{content:[],subtitle:[]})
}

function unShapeShift(p){
if(!mobIds[p])return
if(!api.checkValid(mobIds[p]))return
api.removeFollowingEntityFromPlayer(p,mobIds[p])
delete mobIds[p]
api.setTargetedPlayerSettingForEveryone(p,"opacity",1)
api.setTargetedPlayerSettingForEveryone(p,"nameTagInfo",{})
}

onPlayerAttemptAltAction=(p,x,y,z,b,e)=>{
const held=api.getHeldItem(p)?.attributes?.customDisplayName
if(e){
if(held==="Shape Shift"){
shapeShift(p,e)
return "preventAction"
}
}
if(held==="Un-Shape Shift"){
unShapeShift(p)
return "preventAction"
}
}

onPlayerDamagingMob=(p,m)=>{
if(!mobIds[p])return
if(m===mobIds[p])return "preventDamage"
}

onPlayerJoin=(p)=>{
api.setItemSlot(p,0,"Draugr Rod",/*p*/1,{customDisplayName:"Shape Shift",customAttributes:{enchantmentTier:"Tier 4"}})
api.setItemSlot(p,1,"Cursed Rod",/*p*/1,{customDisplayName:"Un-Shape Shift",customAttributes:{enchantmentTier:"Tier 4"}})
api.sendMessage(p,"Welcome! You have been given your shapeshift tools. Type !help for usage instructions.",{fontWeight:"800",color:"aqua"})
}

onPlayerLeave=(p)=>{
delete mobIds[p]
}

onPlayerChat=(p,m)=>{
if(m!=="!help")return
api.sendMessage(p,[{str:"Here's how to shapeshift:\n",style:{color:"aqua",fontSize:"20px",fontWeight:"bold"}},{str:`1. Right-click the desired mob with your "Shape Shift" item.\n`,style:{color:"gold",fontSize:"10px",fontWeight:"bold"}},{str:`2. Right-click with your "Un-Shape Shift" item to unshapeshift!\n`,style:{color:"gold",fontSize:"10px",fontWeight:"bold"}},{str:"Have fun!",style:{color:"aqua",fontSize:"20px",fontWeight:"bold"}}])
return false
}
