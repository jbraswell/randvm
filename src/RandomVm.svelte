<script>
    import moment from 'moment-timezone';
    import fetchJsonp from 'fetch-jsonp';

    export let randomMeeting = [];
    export let buttonTxt = "Get A Random Meeting";
    export let currentlyRefreshing = false

    function getAdjustedDateTime(meeting_day, meeting_time, meeting_time_zone) {
        let adjustedMeetingDay = parseInt(meeting_day) === 1 ? 7 : parseInt(meeting_day) - 1
        let meeting_date_time_obj;

        if (!meeting_time_zone) {
            meeting_time_zone = "UTC";
        }

        // Get an object that represents the meeting in its time zone
        meeting_date_time_obj = moment.tz(meeting_time_zone).set({
            hour: meeting_time.split(":")[0],
            minute: meeting_time.split(":")[1],
            second: 0
        }).isoWeekday(adjustedMeetingDay);

        // Convert meeting to target (local) time zone
        meeting_date_time_obj = meeting_date_time_obj.clone().tz(moment.tz.guess());

        let now = moment.tz(moment.tz.guess())
        if (now > meeting_date_time_obj) {
            meeting_date_time_obj.add(1, 'weeks');
        }

        return meeting_date_time_obj;
    }

    function getRandomMeeting() {
        buttonTxt = "Finding Meeting..."
        currentlyRefreshing = true
        fetchJsonp('https://bmlt.virtual-na.org/main_server/client_interface/jsonp/?switcher=GetSearchResults&data_field_key=weekday_tinyint,start_time,time_zone,meeting_name,comments')
            .then((response) => response.json())
            .then((meetings) => {
                let results = [];
                if (meetings) {
                    for (let x in meetings) {
                        let meeting_day = meetings[x]['weekday_tinyint'];
                        let meeting_time = meetings[x]['start_time'];
                        let meeting_time_zone = meetings[x]['time_zone'];
                        let start = getAdjustedDateTime(meeting_day, meeting_time, meeting_time_zone);
                        let now = moment.tz(moment.tz.guess());
                        if (start.diff(now, 'minutes') >= 0 && start.diff(now, 'minutes') <= 30) {
                            results.push(
                                {
                                    name: meetings[x]['meeting_name'],
                                    start: start._d.toString(),
                                    link: meetings[x]['comments'],
                                    time: meeting_time,
                                    time_zone: meeting_time_zone
                                });
                        }
                    }
                }
                console.log(results)
                randomMeeting = results[Math.floor(Math.random() * results.length)]
                buttonTxt = "Get A Random Meeting"
                currentlyRefreshing = false
            })
            .catch((ex) => console.log('parsing failed', ex));
    }
</script>

<main>
    <div class="meeting-button">
        <button on:click={getRandomMeeting} disabled={currentlyRefreshing}>{buttonTxt}</button>
    </div>
    <br />
        {#if Object.keys(randomMeeting).length > 0}
            <div class="meeting">
                <b>{randomMeeting.name}</b> <br />
                {randomMeeting.start}<br />
                <a href="{randomMeeting.link}">{randomMeeting.link}</a>
            </div>
        {/if}
    <br />
</main>

<style>
    .meeting-button {
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .meeting {
        font-size: 16px;
        text-align: center
    }
    .meeting a {
        font-size: 14px;
    }
    main {
        margin: 2vh 1vw;
    }
</style>
