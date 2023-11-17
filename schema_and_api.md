// Schema

new Schema({
createdAt: {
type: Date,
default: new Date(),
},
userId: String,
weekSelection: String,
sleepTime: String,
wakeUpTime: String,
totalSleepInMinutes: Number,
changes: [{ change: String }],
});

// APi

export const saveUserOnboardingData = async (req, res) => {
try {
const {
changesArr,
weekSelection,
sleepTime,
wakeUpTime,
timeInBedInHours,
userId,
} = req.body;

    const userSentData = new UserOnBoardingDataModel({
      userId,
      weekSelection,
      sleepTime,
      wakeUpTime,
      totalSleepInMinutes: moment
        .duration(timeInBedInHours, "hours")
        .asMinutes(),
      changes: changesArr,
    });

    userSentData
      .save()
      .then((data) => {
        return res.status(201).json({ message: "Done" });
      })
      .catch((error) => {
        console.log("error", error);
        return res.status(500).json({ message: error.message });
      });

} catch (error) {
console.log("error", error);
return res.status(500).json({ message: error.message });
}
};
