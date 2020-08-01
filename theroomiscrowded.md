# The room is crowded

OH this one was the hardedst of all.

So I tried to solve it at first with just using jsfiddle but then I gave up cuz it was too complex for that and I just kinda retried after open. 

So the hardest part comes now, 

I first made something to get all the data from the file

Then I made a way to parse the data into an array

Then I made a way to convert the arrays into an array of teams with `number[][]` as the type, it was basically [...[startposofteam, endposofteam]...].

Then I used that to create an array of objects which was something like this:
```ts
export interface Diffs {
  diffbehind: number;
  diffinfront: number;
  start: number;
  end: number;
  diff: number;
  middle: number;
  index: number;
  costfront: number;
  costbehind: number;
}
```

and from that I tried to solve it but that was the moment it went downhill and I was under a lot of pressure cuz there wasn;t much time left.

the type of the solution
```ts
interface DiffsStartEnd {
  start: number;
  second: number;
  sum: number;
  cost: number;
  lastmovedL: number;
  lastmovedR: number;
```

The problem was that I went through a bunch of algorithms which all needed something new and the more I improved them the more I knew I needed to add a lot more to them.
I dont have the code form jsfiddle but I have the code I wrote today.

First I tried just shifting the teams to the left but I wansnt taking into consideration the cost,

Then I tried making an algo that shifts the costs based on which cost is bigger, the one on the left part or the one on the right aprt,

but then I realized thatt my method was bad because I was just shifting each team once not every time I needed to shift them

Well everything was good for reference and I collected all the data and put it into a file and then I used an online convertor to convert the json data into an excel file

And being too stressed and with 1 hour left I decided that the code wont take me anywhere cuz I wont have the time to finish it and with all the data in the excell I started manually finding the solution

the first hint was the gap between 35 and 36

and then looking at the costs I tried avoiding moving teamn 20 

then I tried a bunch of other stuff like shifting only the teams on the right of 35 or on the left but no success

then I actually applied what my alg was supposed to do and that was to start from the most left part and the most right part and shift the teams

I calculated the cost which was wrong but it was a good reference and it was all i had and making another aalg for calculating the correct cost wouldve been just like making one for solving the problem which is too long

so I tried comparing the cost on the left of the 35-36 gap and the cost on the right and I actually put more teams in there from the start

I began eliminating one by one depending on the cost and the size of the team

and also comparing the costs I tried to see if adding one team and eleminating another one at the ends will get me a lower cost 

and it did 

and so I kinda submited like 20 mins before or less

all the data collected the top part is kinda wrong

https://prnt.sc/tsgqyu

the bottom part is what I used in the last hour

https://prnt.sc/tsgr6b

as you can see I started with teams from 22 to 48 and slowly eliminated them
I hvae the total angle which was correct and the total cost which was wrong but at least it was a good reference
on the ver right the top number is the top part of the cost and the bottom one is the bottom part of the cost

the teams are arranged from 1 to 49 and shifting them to the right meant that I shift them counter clockwise 
I highlighted with green where I kinda wanted to start and end before trying the part from the right

the calc thing top right wasnt really used I made it to add 2 numbers but then I realized I dont need it 


# and now the code

## the main file

```ts
import { readFile, writeFile } from "fs";

import { shiftTeams } from "./methods/startendteams";
import { getdiffs } from "./methods/creatteamsinfo";
import { getAllDiffsUntil } from "./methods/getalldiffsuntil";

// const file = "./example.in";
const file = "./teams.in";

const main = (data: Array<number | number[]>) => {
  const n = data[0] as number;
  const teamSize = data[data.length - 1] as number;
  const teams = data.slice(1, data.length - 1);
  const shiftedTeams = shiftTeams(teams);
  const info = getdiffs(shiftedTeams);
  const diffs = getAllDiffsUntil(info, teamSize, n);
  const sortedDiffs = diffs.sort((a, b) => a.cost - b.cost);
  const sortedAnswers = convertIntoAnswer(
    diffs.sort((a, b) => a.cost - b.cost)
  );

  // console.log(n, teamSize);
  // console.log(teams);
  // console.log(shiftedTeams);
  console.log(info);
  console.log(sortedDiffs);

  console.log(sortedAnswers);

  // writeFile(
  //   "result.txt",

  //   JSON.stringify(info),
  //   function (err) {
  //     if (err) throw err;
  //     console.log("Saved!");
  //   }
  // );
};

readFile(file, "utf8", function (err, data) {
  if (err) {
    console.log(err);
  }
  //   console.log(data);

  const newData1 = data.split("\r");
  //   console.log(newData1);

  const newData = newData1.map((data, indix) => {
    if (data.startsWith("\n")) {
      const dat = data.slice(1, data.length);
      //   console.log("1", dat);
      return dat.split(" ").map((data) => parseFloat(data));
    } else return parseFloat(data);
  });
  newData[newData.length - 1] = newData[newData.length - 1][0];
  //   console.log(newData);

  main(newData);
});

const convertIntoAnswer = (data) => {
  return data.map((value) => {
    return `{${value.lastmovedR}, ${value.start}, ${value.second}, ${value.lastmovedL}}`;
  });
};


```

# creatteamsinfo.ts

```ts

export interface Diffs {
  diffbehind: number;
  diffinfront: number;
  start: number;
  end: number;
  diff: number;
  middle: number;
  index: number;
  costfront: number;
  costbehind: number;
}

export const getdiffs = (teams: number[][]): Diffs[] => {
  let allteams = teams.map((v: number[]) => [...v]);
  let result: Diffs[] = allteams.map((value, index) => {
    if (index === 0) {
      const diffbehind =
        Math.floor(
          (360 - allteams[allteams.length - 1][1] + allteams[0][0]) * 1000
        ) / 1000;

      const diffinfront = Math.floor((allteams[1][0] - value[1]) * 1000) / 1000;

      const diff = value[1] - value[0];
      const middle = Math.floor((value[0] + diff / 2) * 1000) / 1000;
      let costfront = Math.floor(diff * diffinfront * 1000) / 1000;
      let costbehind = Math.floor(diff * diffbehind * 1000) / 1000;
      let i = index - 1;

      while (costbehind === 0) {
        if (i < 0) {
          i = allteams.length - 1;
        } else if (i >= allteams.length) {
          i = 0;
        }

        let nexti = i - 1;
        if (nexti < 0) {
          nexti = allteams.length - 1;
        } else if (nexti >= allteams.length) {
          nexti = 0;
        }

        let newdiffbehind = allteams[i][0] - allteams[nexti][1];
        if (newdiffbehind < 0) {
          newdiffbehind = newdiffbehind * -1;
        }
        costbehind = Math.floor(diff * newdiffbehind * 1000) / 1000;
        i--;
      }

      i = index + 1;
      while (costfront === 0) {
        if (i < 0) {
          i = allteams.length - 1;
        } else if (i >= allteams.length) {
          i = 0;
        }

        let nexti = i + 1;
        if (nexti < 0) {
          nexti = allteams.length - 1;
        } else if (nexti >= allteams.length) {
          nexti = 0;
        }

        let newdifffront = allteams[nexti][0] - allteams[i][1];
        if (newdifffront < 0) {
          newdifffront = newdifffront * -1;
        }
        costfront = Math.floor(diff * newdifffront * 1000) / 1000;
        i++;
      }

      const start = value[0];
      const end = value[1];
      return {
        index: index + 1,
        diffbehind,
        diffinfront,
        start,
        end,
        diff: Math.floor(diff * 1000) / 1000,
        middle,
        costfront,
        costbehind,
      };
    } else if (index === allteams.length - 1) {
      const diffbehind =
        Math.floor((allteams[index][1] - value[0]) * 1000) / 1000;

      const diffinfront =
        Math.floor(
          (360 - allteams[allteams.length - 1][1] + allteams[0][0]) * 1000
        ) / 1000;

      const diff = value[1] - value[0];
      const middle = Math.floor((value[0] + diff / 2) * 1000) / 1000;

      let costfront = Math.floor(diff * diffinfront * 1000) / 1000;
      let costbehind = Math.floor(diff * diffbehind * 1000) / 1000;
      let i = index - 1;

      while (costbehind === 0) {
        if (i < 0) {
          i = allteams.length - 1;
        } else if (i >= allteams.length) {
          i = 0;
        }

        let nexti = i - 1;
        if (nexti < 0) {
          nexti = allteams.length - 1;
        } else if (nexti >= allteams.length) {
          nexti = 0;
        }

        let newdiffbehind = allteams[i][0] - allteams[nexti][1];
        if (newdiffbehind < 0) {
          newdiffbehind = newdiffbehind * -1;
        }
        costbehind = Math.floor(diff * newdiffbehind * 1000) / 1000;
        i--;
      }
      i = index + 1;
      while (costfront === 0) {
        if (i < 0) {
          i = allteams.length - 1;
        } else if (i >= allteams.length) {
          i = 0;
        }

        let nexti = i + 1;
        if (nexti < 0) {
          nexti = allteams.length - 1;
        } else if (nexti >= allteams.length) {
          nexti = 0;
        }

        let newdifffront = allteams[nexti][0] - allteams[i][1];
        if (newdifffront < 0) {
          newdifffront = newdifffront * -1;
        }
        costfront = Math.floor(diff * newdifffront * 1000) / 1000;
        i++;
      }

      const start = value[0];
      const end = value[1];
      return {
        index: index + 1,

        diffbehind,
        diffinfront,
        start,
        end,
        diff: Math.floor(diff * 1000) / 1000,
        middle,
        costfront,
        costbehind,
      };
    }
    const diffbehind =
      Math.floor((value[0] - allteams[index - 1][1]) * 1000) / 1000;

    const diffinfront =
      Math.floor((allteams[index + 1][0] - value[1]) * 1000) / 1000;
    const diff = value[1] - value[0];
    const middle = Math.floor((value[0] + diff / 2) * 1000) / 1000;

    let costfront = Math.floor(diff * diffinfront * 1000) / 1000;
    let costbehind = Math.floor(diff * diffbehind * 1000) / 1000;
    let i = index - 1;

    while (costbehind === 0) {
      if (i < 0) {
        i = allteams.length - 1;
      } else if (i >= allteams.length) {
        i = 0;
      }

      let nexti = i - 1;
      if (nexti < 0) {
        nexti = allteams.length - 1;
      } else if (nexti >= allteams.length) {
        nexti = 0;
      }

      let newdiffbehind = allteams[i][0] - allteams[nexti][1];
      if (newdiffbehind < 0) {
        newdiffbehind = newdiffbehind * -1;
      }
      costbehind = Math.floor(diff * newdiffbehind * 1000) / 1000;
      i--;
    }
    i = index + 1;
    while (costfront === 0) {
      if (i < 0) {
        i = allteams.length - 1;
      } else if (i >= allteams.length) {
        i = 0;
      }

      let nexti = i + 1;
      if (nexti < 0) {
        nexti = allteams.length - 1;
      } else if (nexti >= allteams.length) {
        nexti = 0;
      }

      let newdifffront = allteams[nexti][0] - allteams[i][1];
      if (newdifffront < 0) {
        newdifffront = newdifffront * -1;
      }
      costfront = Math.floor(diff * newdifffront * 1000) / 1000;
      i++;
    }

    const start = value[0];
    const end = value[1];

    return {
      index: index + 1,

      diffbehind,
      diffinfront,
      start,
      end,
      diff: Math.floor(diff * 1000) / 1000,
      middle,
      costfront,
      costbehind,
    };
  });
  return result;
};

```

# getalldiffsuntil.ts

```ts
import { Diffs } from "./creatteamsinfo";

interface DiffsStartEnd {
  start: number;
  second: number;
  sum: number;
  cost: number;
  lastmovedL: number;
  lastmovedR: number;
}

export const getAllDiffsUntil = (
  teams: Diffs[],
  size: number,
  nrteams: number
): DiffsStartEnd[] => {
  let allteams = teams.map((v: Diffs) => Object.assign(v)) as Diffs[];

  let arrayofDiffs: DiffsStartEnd[];

  arrayofDiffs = allteams.map((value, index) => {
    let sum = 0;
    let cost = 0;
    let indexLeft = index - 30;
    let indexRight = index + 30;
    let second;

    if (indexLeft < 0) {
      indexLeft = nrteams - 1;
    }
    if (indexRight >= nrteams) {
      indexRight = 0;
    }

    sum +=
      Math.floor(allteams[indexLeft].diffinfront * 1000) / 1000 +
      Math.floor(allteams[indexRight].diffbehind * 1000) / 1000;

    if (allteams[indexLeft].costbehind > allteams[indexRight].costfront) {
      cost += Math.floor(allteams[indexRight].costbehind * 1000) / 1000;
      second = indexRight + 1;
      indexRight++;
    } else {
      cost += Math.floor(allteams[indexRight].costfront * 1000) / 1000;
      second = indexLeft - 1;
      indexLeft--;
    }

    if (second < 0) {
      second = nrteams;
    } else if (second > nrteams) {
      second = 1;
    }

    while (sum < size) {
      if (indexLeft < 0) {
        indexLeft = nrteams - 1;
      }
      if (indexRight >= nrteams) {
        indexRight = 0;
      }
      //   if (indexLeft > index || indexRight < index) break;
      let costbehind = 0;
      let newindexleft = indexLeft;
      let lastmoved;
      let newindex;
      newindex = indexRight - 1;
      if (newindex < 0) {
        newindex = nrteams - 1;
      }
      if (newindex >= nrteams) {
        newindex = 0;
      }
      lastmoved = Object.assign(allteams[newindex]);
      while (newindexleft < index) {
        costbehind +=
          Math.floor(
            allteams[newindexleft].diff *
              (allteams[newindexleft].start - lastmoved.end) *
              1000
          ) / 1000;
        let { end, ...rest } = lastmoved;
        let newLastmoved = {
          end: lastmoved.end + allteams[newindexleft].diff,
          ...rest,
        };

        lastmoved = Object.assign(newLastmoved);

        newindexleft++;
      }

      let costfront = 0;
      let newindexright = indexRight;
      newindex = indexRight + 1;
      if (newindex < 0) {
        newindex = nrteams - 1;
      }
      if (newindex >= nrteams) {
        newindex = 0;
      }
      lastmoved = Object.assign(allteams[newindex]);

      while (newindexright > index) {
        costbehind +=
          Math.floor(
            allteams[newindexright].diff *
              (lastmoved.start - allteams[newindexright].end) *
              1000
          ) / 1000;
        let { start, ...rest } = lastmoved;
        let newLastmoved = {
          start: lastmoved.start - allteams[newindexright].diff,
          ...rest,
        };

        lastmoved = Object.assign(newLastmoved);

        newindexright--;
      }

      if (costbehind > costfront) {
        cost += costfront;
        sum += Math.floor(allteams[indexRight].diffinfront * 1000) / 1000;

        indexRight++;
      } else {
        cost += costbehind;
        sum += Math.floor(allteams[indexLeft].diffbehind * 1000) / 1000;

        indexLeft--;
      }
    }

    indexLeft++;
    indexRight++;
    let start = index + 1;
    second++;
    if (second < start) {
      second++;
      start--;
    }

    if (indexLeft < 0) {
      indexLeft = nrteams;
    }
    if (indexRight > nrteams) {
      indexRight = 1;
    }

    if (start > nrteams) {
      start = nrteams;
    } else if (start === 0) {
      start = 1;
    }
    if (second > nrteams) {
      second = nrteams;
    } else if (second === 0) {
      second = 1;
    }

    return {
      start,
      second,
      sum,
      cost,
      lastmovedL: indexRight,
      lastmovedR: indexLeft,
    };
  });
  return arrayofDiffs;
};
```

# startendteams.ts

```ts
export const shiftTeams = (teams: Array<number[] | number>): number[][] => {
  let result = teams.map((v: number[]) => [...v]);
  // console.log(result);

  // result[0][0] = Math.floor(()*1000)/1000
  // result[0][0] = Math.floor(()*1000)/1000

  for (let i = 0; i < result.length; i++) {
    const team = [result[i][0], result[i][1]];

    result[i][0] = Math.floor((team[1] - team[0] / 2) * 1000) / 1000;
    result[i][1] = Math.floor((team[1] + team[0] / 2) * 1000) / 1000;
  }

  //  console.log(teams);

  //      for (let i = 1; i < teams.length; i++) {
  //         const team = [teams[i][0], teams[i][1]]
  //         const prevTeam = [teams[i-1][0], teams[i-1][1]]
  //         const diff = Math.floor((team[1] - team[0])*1000)/1000;

  //         if(team[0] - prevTeam[1] >= yourTeam){
  //         console.log("solution",teams)
  //         console.log(i+1,i+1,i,i)
  //         }
  // }
  return result;
};
```


### And now a few screenshots from what the outputs are

(I mean the console logs in the main file)

the log with `n`, `teamsize` and the `teams`
https://prnt.sc/tsgtlj

the `shiftedteams` log which is actually me having the teams with start and end
https://prnt.sc/tsgtsd

small part of `info` log
https://prnt.sc/tsgu1h

part of `sorteddiffs` log, which is broken because I didnt finish the alg it was left at the "imma do it manually" part
https://prnt.sc/tsgufc

and finallly `sortedanswers` log
https://prnt.sc/tsgune

The `info` log is the one that I put in excell to solve the stuff manually.
