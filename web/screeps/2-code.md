# 代码

1. TOC
{:toc}

## 入门代码

###1、生成creeps
//生成creep
StructureSpawn.spawnCreep

//生成creeps
//Spawn1这个中心生成一个[工作，携带，移动]的名为Harvester1的creep
Game.spawns['Spawn1'].spawnCreep([WORK,CARRY,MOVE],'Harvester1');

//选中creep
Game.creeps['Harvester1']

RoomObject.room

//Room指的是对象，creep要调用room时用的是小写的room
Room.find

Creep.moveTo

Creep.harvest

//把能量传回spawn
Creep.transfer

Creep.store

//Creep是否采集完能源没有空闲
Creep.store.getFreeCapacity()

for...in loops


//creep移向能源
module.exports.loop = function () {
    var creep = Game.creeps['Harvester1'];
    //FIND_SOURCES是常量
    var sources = creep.room.find(FIND_SOURCES);
    if(creep.harvest(sources[0]) == ERR_NOT_IN_RANGE) {
        creep.moveTo(sources[0]);
    }
}

//Creep自己来回采集能源
module.exports.loop = function () {
    var creep = Game.creeps['Harvester1'];
    if(creep.store.getFreeCapacity() > 0) {
        var sources = creep.room.find(FIND_SOURCES);
        if(creep.harvest(sources[0]) == ERR_NOT_IN_RANGE) {
            creep.moveTo(sources[0]);
        }
    }
    else {
        if( creep.transfer(Game.spawns['Spawn1'], RESOURCE_ENERGY) == ERR_NOT_IN_RANGE ) {
            creep.moveTo(Game.spawns['Spawn1']);
        }
    }
}

module.exports.loop = function () {
    //循环遍历
    for(var name in Game.creeps) {
        var creep = Game.creeps[name];
        if(creep.store.getFreeCapacity() > 0) {
            var sources = creep.room.find(FIND_SOURCES);
            if(creep.harvest(sources[0]) == ERR_NOT_IN_RANGE) {
                creep.moveTo(sources[0]);
            }
        }
        else {
            if(creep.transfer(Game.spawns['Spawn1'], RESOURCE_ENERGY) == ERR_NOT_IN_RANGE) {
                creep.moveTo(Game.spawns['Spawn1']);
            }
        }
    }
}

//在左边的script页面中新建一个model封装方法
module.exports = {
var roleHarvester = {
    /** @param {Creep} creep **/
    //该model的run方法入参为Creeep
    run: function(creep) {
	    if(creep.store.getFreeCapacity() > 0) {
            var sources = creep.room.find(FIND_SOURCES);
            if(creep.harvest(sources[0]) == ERR_NOT_IN_RANGE) {
                creep.moveTo(sources[0]);
            }
        }
        else {
            if(creep.transfer(Game.spawns['Spawn1'], RESOURCE_ENERGY) == ERR_NOT_IN_RANGE) {
                creep.moveTo(Game.spawns['Spawn1']);
            }
        }
	}
};

module.exports = roleHarvester;
}

//封装好方法后，在main方法中进行循环调用
//先通过require创建刚刚新建的model
var roleHarvester = require('role.harvester');
module.exports.loop = function () {
    for(var name in Game.creeps) {
        var creep = Game.creeps[name];
        roleHarvester.run(creep);
    }
}
## 页面介绍



## 快捷键

