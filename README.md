# StudyTracker
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Study & Screen Time Tracker</title>
    <script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body>
    <div id="root"></div>
    
    <script type="text/babel">
        const { useState, useEffect } = React;
        
        // Lucide Icons as inline SVG components
        const BookOpen = ({className}) => <svg className={className} fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" /></svg>;
        const Smartphone = ({className}) => <svg className={className} fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M12 18h.01M8 21h8a2 2 0 002-2V5a2 2 0 00-2-2H8a2 2 0 00-2 2v14a2 2 0 002 2z" /></svg>;
        const Plus = ({className}) => <svg className={className} fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M12 4v16m8-8H4" /></svg>;
        const Play = ({className}) => <svg className={className} fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z" /><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>;
        const Pause = ({className}) => <svg className={className} fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M10 9v6m4-6v6m7-3a9 9 0 11-18 0 9 9 0 0118 0z" /></svg>;
        const Target = ({className}) => <svg className={className} fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8zm0-14c-3.31 0-6 2.69-6 6s2.69 6 6 6 6-2.69 6-6-2.69-6-6-6z" /></svg>;
        const TrendingDown = ({className}) => <svg className={className} fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M13 17h8m0 0V9m0 8l-8-8-4 4-6-6" /></svg>;
        const Award = ({className}) => <svg className={className} fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M9 12l2 2 4-4M7.835 4.697a3.42 3.42 0 001.946-.806 3.42 3.42 0 014.438 0 3.42 3.42 0 001.946.806 3.42 3.42 0 013.138 3.138 3.42 3.42 0 00.806 1.946 3.42 3.42 0 010 4.438 3.42 3.42 0 00-.806 1.946 3.42 3.42 0 01-3.138 3.138 3.42 3.42 0 00-1.946.806 3.42 3.42 0 01-4.438 0 3.42 3.42 0 00-1.946-.806 3.42 3.42 0 01-3.138-3.138 3.42 3.42 0 00-.806-1.946 3.42 3.42 0 010-4.438 3.42 3.42 0 00.806-1.946 3.42 3.42 0 013.138-3.138z" /></svg>;
        const FileText = ({className}) => <svg className={className} fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" /></svg>;
        const Edit = ({className}) => <svg className={className} fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z" /></svg>;
        const Trash2 = ({className}) => <svg className={className} fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" /></svg>;
        const Share2 = ({className}) => <svg className={className} fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M8.684 13.342C8.886 12.938 9 12.482 9 12c0-.482-.114-.938-.316-1.342m0 2.684a3 3 0 110-2.684m0 2.684l6.632 3.316m-6.632-6l6.632-3.316m0 0a3 3 0 105.367-2.684 3 3 0 00-5.367 2.684zm0 9.316a3 3 0 105.368 2.684 3 3 0 00-5.368-2.684z" /></svg>;
        const X = ({className}) => <svg className={className} fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M6 18L18 6M6 6l12 12" /></svg>;

        function StudyScreenTimeApp() {
          const [view, setView] = useState('today');
          const [studySessions, setStudySessions] = useState([]);
          const [screenTimeLog, setScreenTimeLog] = useState([]);
          const [activeTimer, setActiveTimer] = useState(null);
          const [timerSeconds, setTimerSeconds] = useState(0);
          const [dailyGoals, setDailyGoals] = useState({
            studyMinutes: 120,
            screenTimeMinutes: 120
          });
          const [newScreenTime, setNewScreenTime] = useState('');
          const [notes, setNotes] = useState([]);
          const [showNoteModal, setShowNoteModal] = useState(false);
          const [currentNote, setCurrentNote] = useState({ title: '', content: '', subject: '' });

          useEffect(() => {
            const saved = localStorage.getItem('studyData');
            if (saved) {
              const data = JSON.parse(saved);
              setStudySessions(data.studySessions || []);
              setScreenTimeLog(data.screenTimeLog || []);
              setDailyGoals(data.dailyGoals || { studyMinutes: 120, screenTimeMinutes: 120 });
              setNotes(data.notes || []);
            }
          }, []);

          useEffect(() => {
            localStorage.setItem('studyData', JSON.stringify({
              studySessions,
              screenTimeLog,
              dailyGoals,
              notes
            }));
          }, [studySessions, screenTimeLog, dailyGoals, notes]);

          useEffect(() => {
            let interval;
            if (activeTimer) {
              interval = setInterval(() => {
                setTimerSeconds(prev => prev + 1);
              }, 1000);
            }
            return () => clearInterval(interval);
          }, [activeTimer]);

          const today = new Date().toDateString();

          const startStudySession = (subject) => {
            setActiveTimer({ subject, startTime: Date.now() });
            setTimerSeconds(0);
          };

          const stopStudySession = () => {
            if (activeTimer) {
              setStudySessions([...studySessions, {
                id: Date.now(),
                subject: activeTimer.subject,
                duration: timerSeconds,
                date: today,
                timestamp: new Date().toISOString()
              }]);
              setActiveTimer(null);
              setTimerSeconds(0);
            }
          };

          const logScreenTime = () => {
            if (newScreenTime && !isNaN(newScreenTime)) {
              setScreenTimeLog([...screenTimeLog, {
                id: Date.now(),
                minutes: parseInt(newScreenTime),
                date: today,
                timestamp: new Date().toISOString()
              }]);
              setNewScreenTime('');
            }
          };

          const saveNote = () => {
            if (currentNote.title.trim() && currentNote.content.trim()) {
              if (currentNote.id) {
                setNotes(notes.map(n => n.id === currentNote.id ? currentNote : n));
              } else {
                setNotes([...notes, {
                  ...currentNote,
                  id: Date.now(),
                  date: today,
                  timestamp: new Date().toISOString()
                }]);
              }
              setCurrentNote({ title: '', content: '', subject: '' });
              setShowNoteModal(false);
            }
          };

          const deleteNote = (id) => {
            setNotes(notes.filter(n => n.id !== id));
          };

          const editNote = (note) => {
            setCurrentNote(note);
            setShowNoteModal(true);
          };

          const getTodayStudyMinutes = () => {
            return studySessions
              .filter(s => s.date === today)
              .reduce((total, s) => total + Math.floor(s.duration / 60), 0);
          };

          const getTodayScreenTime = () => {
            return screenTimeLog
              .filter(s => s.date === today)
              .reduce((total, s) => total + s.minutes, 0);
          };

          const formatTime = (seconds) => {
            const hrs = Math.floor(seconds / 3600);
            const mins = Math.floor((seconds % 3600) / 60);
            const secs = seconds % 60;
            if (hrs > 0) {
              return `${hrs}:${mins.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
            }
            return `${mins}:${secs.toString().padStart(2, '0')}`;
          };

          const getWeeklyStats = () => {
            const last7Days = [];
            for (let i = 6; i >= 0; i--) {
              const date = new Date();
              date.setDate(date.getDate() - i);
              const dateStr = date.toDateString();
              
              const studyMins = studySessions
                .filter(s => s.date === dateStr)
                .reduce((total, s) => total + Math.floor(s.duration / 60), 0);
              
              const screenMins = screenTimeLog
                .filter(s => s.date === dateStr)
                .reduce((total, s) => total + s.minutes, 0);
              
              last7Days.push({
                date: date.toLocaleDateString('en-US', { weekday: 'short' }),
                study: studyMins,
                screen: screenMins
              });
            }
            return last7Days;
          };

          const todayStudy = getTodayStudyMinutes();
          const todayScreen = getTodayScreenTime();
          const studyProgress = Math.min(100, Math.round((todayStudy / dailyGoals.studyMinutes) * 100));
          const screenProgress = Math.min(100, Math.round((todayScreen / dailyGoals.screenTimeMinutes) * 100));
          const weeklyStats = getWeeklyStats();

          const quickStudyTopics = ['Mathematics', 'Science', 'Languages', 'History', 'Programming', 'Other'];

          return (
            <div className="min-h-screen bg-gradient-to-br from-indigo-50 to-purple-50 pb-20">
              <div className="bg-gradient-to-r from-indigo-600 to-purple-600 text-white p-6 rounded-b-3xl shadow-lg">
                <h1 className="text-2xl font-bold mb-1">Study & Screen Time</h1>
                <p className="text-indigo-100 text-sm">{new Date().toLocaleDateString('en-US', { weekday: 'long', month: 'long', day: 'numeric' })}</p>
                
                <div className="mt-6 grid grid-cols-2 gap-4">
                  <div className="bg-white/20 backdrop-blur rounded-2xl p-4">
                    <div className="flex items-center gap-2 mb-2">
                      <BookOpen className="w-5 h-5" />
                      <span className="text-sm font-medium">Study Today</span>
                    </div>
                    <p className="text-3xl font-bold">{todayStudy}m</p>
                    <div className="mt-2 bg-white/20 rounded-full h-2">
                      <div className="bg-white h-full rounded-full transition-all" style={{ width: `${studyProgress}%` }} />
                    </div>
                    <p className="text-xs mt-1 text-indigo-100">Goal: {dailyGoals.studyMinutes}m</p>
                  </div>
                  
                  <div className="bg-white/20 backdrop-blur rounded-2xl p-4">
                    <div className="flex items-center gap-2 mb-2">
                      <Smartphone className="w-5 h-5" />
                      <span className="text-sm font-medium">Screen Time</span>
                    </div>
                    <p className="text-3xl font-bold">{todayScreen}m</p>
                    <div className="mt-2 bg-white/20 rounded-full h-2">
                      <div className={`h-full rounded-full transition-all ${screenProgress > 100 ? 'bg-red-400' : 'bg-white'}`} style={{ width: `${Math.min(screenProgress, 100)}%` }} />
                    </div>
                    <p className="text-xs mt-1 text-indigo-100">Limit: {dailyGoals.screenTimeMinutes}m</p>
                  </div>
                </div>
              </div>

              <div className="flex gap-2 p-4 overflow-x-auto">
                <button onClick={() => setView('today')} className={`py-2 px-4 rounded-xl font-medium transition-all whitespace-nowrap ${view === 'today' ? 'bg-indigo-600 text-white shadow-md' : 'bg-white text-gray-700'}`}>Today</button>
                <button onClick={() => setView('notes')} className={`py-2 px-4 rounded-xl font-medium transition-all whitespace-nowrap ${view === 'notes' ? 'bg-indigo-600 text-white shadow-md' : 'bg-white text-gray-700'}`}>Notes</button>
                <button onClick={() => setView('stats')} className={`py-2 px-4 rounded-xl font-medium transition-all whitespace-nowrap ${view === 'stats' ? 'bg-indigo-600 text-white shadow-md' : 'bg-white text-gray-700'}`}>Statistics</button>
                <button onClick={() => setView('goals')} className={`py-2 px-4 rounded-xl font-medium transition-all whitespace-nowrap ${view === 'goals' ? 'bg-indigo-600 text-white shadow-md' : 'bg-white text-gray-700'}`}>Goals</button>
              </div>

              <div className="p-4 space-y-4">
                {view === 'today' && (
                  <>
                    {activeTimer && (
                      <div className="bg-gradient-to-r from-green-500 to-emerald-500 text-white rounded-3xl p-6 shadow-lg">
                        <div className="flex items-center justify-between mb-4">
                          <div className="flex items-center gap-2">
                            <div className="w-3 h-3 bg-white rounded-full animate-pulse" />
                            <span className="font-medium">Studying: {activeTimer.subject}</span>
                          </div>
                        </div>
                        <div className="text-5xl font-bold text-center mb-6">{formatTime(timerSeconds)}</div>
                        <button onClick={stopStudySession} className="w-full bg-white text-green-600 py-4 rounded-2xl font-bold text-lg shadow-md hover:shadow-lg transition-all">
                          <Pause className="inline w-5 h-5 mr-2" />Stop Session
                        </button>
                      </div>
                    )}

                    {!activeTimer && (
                      <div className="bg-white rounded-3xl p-6 shadow-sm">
                        <h2 className="text-lg font-bold text-gray-800 mb-4 flex items-center gap-2">
                          <BookOpen className="w-5 h-5 text-indigo-600" />Start Study Session
                        </h2>
                        <div className="grid grid-cols-2 gap-3">
                          {quickStudyTopics.map(topic => (
                            <button key={topic} onClick={() => startStudySession(topic)} className="bg-indigo-50 hover:bg-indigo-100 text-indigo-700 py-3 px-4 rounded-xl font-medium transition-all flex items-center justify-center gap-2">
                              <Play className="w-4 h-4" />{topic}
                            </button>
                          ))}
                        </div>
                      </div>
                    )}

                    <div className="bg-white rounded-3xl p-6 shadow-sm">
                      <h2 className="text-lg font-bold text-gray-800 mb-4 flex items-center gap-2">
                        <Smartphone className="w-5 h-5 text-purple-600" />Log Screen Time
                      </h2>
                      <div className="flex gap-3">
                        <input type="number" value={newScreenTime} onChange={(e) => setNewScreenTime(e.target.value)} placeholder="Minutes" className="flex-1 p-3 border-2 border-gray-200 rounded-xl outline-none focus:border-purple-500" />
                        <button onClick={logScreenTime} className="bg-purple-600 text-white px-6 rounded-xl font-medium hover:bg-purple-700 transition-all">
                          <Plus className="w-5 h-5" />
                        </button>
                      </div>
                      <p className="text-sm text-gray-500 mt-2">Track your daily phone/computer usage</p>
                    </div>

                    {studySessions.filter(s => s.date === today).length > 0 && (
                      <div className="bg-white rounded-3xl p-6 shadow-sm">
                        <h2 className="text-lg font-bold text-gray-800 mb-4">Today's Study Sessions</h2>
                        <div className="space-y-2">
                          {studySessions.filter(s => s.date === today).reverse().map(session => (
                            <div key={session.id} className="flex justify-between items-center p-3 bg-indigo-50 rounded-xl">
                              <div>
                                <p className="font-medium text-gray-800">{session.subject}</p>
                                <p className="text-sm text-gray-500">{new Date(session.timestamp).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })}</p>
                              </div>
                              <div className="text-right">
                                <p className="font-bold text-indigo-600">{Math.floor(session.duration / 60)}m</p>
                              </div>
                            </div>
                          ))}
                        </div>
                      </div>
                    )}
                  </>
                )}

                {view === 'notes' && (
                  <>
                    <div className="flex justify-between items-center mb-4">
                      <h2 className="text-xl font-bold text-gray-800">Study Notes</h2>
                      <button onClick={() => { setCurrentNote({ title: '', content: '', subject: '' }); setShowNoteModal(true); }} className="bg-indigo-600 text-white p-3 rounded-full shadow-lg">
                        <Plus className="w-5 h-5" />
                      </button>
                    </div>

                    {notes.length === 0 ? (
                      <div className="bg-white rounded-3xl p-12 text-center shadow-sm">
                        <FileText className="w-16 h-16 mx-auto mb-4 text-gray-300" />
                        <p className="text-gray-500 mb-2">No notes yet</p>
                        <p className="text-sm text-gray-400">Start taking notes for your studies!</p>
                      </div>
                    ) : (
                      <div className="space-y-3">
                        {notes.slice().reverse().map(note => (
                          <div key={note.id} className="bg-white rounded-2xl p-5 shadow-sm">
                            <div className="flex justify-between items-start mb-3">
                              <div className="flex-1">
                                <h3 className="font-bold text-gray-800 text-lg mb-1">{note.title}</h3>
                                {note.subject && (
                                  <span className="inline-block bg-indigo-100 text-indigo-700 text-xs px-3 py-1 rounded-full">{note.subject}</span>
                                )}
                              </div>
                              <div className="flex gap-2">
                                <button onClick={() => editNote(note)} className="text-gray-400 hover:text-indigo-600 p-1">
                                  <Edit className="w-5 h-5" />
                                </button>
                                <button onClick={() => { if (navigator.share) { navigator.share({ title: note.title, text: note.content }); } }} className="text-gray-400 hover:text-green-600 p-1">
                                  <Share2 className="w-5 h-5" />
                                </button>
                                <button onClick={() => deleteNote(note.id)} className="text-gray-400 hover:text-red-600 p-1">
                                  <Trash2 className="w-5 h-5" />
                                </button>
                              </div>
                            </div>
                            <p className="text-gray-600 whitespace-pre-wrap leading-relaxed">{note.content}</p>
                            <p className="text-xs text-gray-400 mt-3">
                              {new Date(note.timestamp).toLocaleDateString()} at {new Date(note.timestamp).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })}
                            </p>
                          </div>
                        ))}
                      </div>
                    )}
                  </>
                )}

                {view === 'stats' && (
                  <>
                    <div className="bg-white rounded-3xl p-6 shadow-sm">
                      <h2 className="text-lg font-bold text-gray-800 mb-4">7-Day Overview</h2>
                      <div className="space-y-3">
                        {weeklyStats.map((day, idx) => (
                          <div key={idx}>
                            <div className="flex justify-between items-center mb-1">
                              <span className="text-sm font-medium text-gray-600">{day.date}</span>
                              <div className="flex gap-4 text-sm">
                                <span className="text-indigo-600 font-medium">{day.study}m study</span>
                                <span className="text-purple-600 font-medium">{day.screen}m screen</span>
                              </div>
                            </div>
                            <div className="flex gap-2 h-2">
                              <div className="flex-1 bg-indigo-100 rounded-full overflow-hidden">
                                <div className="bg-indigo-600 h-full rounded-full" style={{ width: `${Math.min(100, (day.study / dailyGoals.studyMinutes) * 100)}%` }} />
                              </div>
                              <div className="flex-1 bg-purple-100 rounded-full overflow-hidden">
                                <div className={`h-full rounded-full ${day.screen > dailyGoals.screenTimeMinutes ? 'bg-red-500' : 'bg-purple-600'}`} style={{ width: `${Math.min(100, (day.screen / dailyGoals.screenTimeMinutes) * 100)}%` }} />
                              </div>
                            </div>
                          </div>
                        ))}
                      </div>
                    </div>

                    <div className="grid grid-cols-2 gap-4">
                      <div className="bg-gradient-to-br from-indigo-500 to-indigo-600 text-white rounded-3xl p-6 shadow-sm">
                        <Award className="w-8 h-8 mb-2 opacity-80" />
                        <p className="text-3xl font-bold mb-1">{studySessions.length}</p>
                        <p className="text-sm opacity-90">Total Sessions</p>
                      </div>
                      
                      <div className="bg-gradient-to-br from-purple-500 to-purple-600 text-white rounded-3xl p-6 shadow-sm">
                        <TrendingDown className="w-8 h-8 mb-2 opacity-80" />
                        <p className="text-3xl font-bold mb-1">{Math.round(screenTimeLog.reduce((sum, log) => sum + log.minutes, 0) / 60)}h</p>
                        <p className="text-sm opacity-90">Total Screen Time</p>
                      </div>
                    </div>
                  </>
                )}

                {view === 'goals' && (
                  <div className="bg-white rounded-3xl p-6 shadow-sm">
                    <h2 className="text-lg font-bold text-gray-800 mb-6 flex items-center gap-2">
                      <Target className="w-5 h-5 text-indigo-600" />Daily Goals
                    </h2>
                    
                    <div className="space-y-6">
                      <div>
                        <label className="block text-sm font-medium text-gray-700 mb-2">Study Goal (minutes per day)</label>
                        <input type="number" value={dailyGoals.studyMinutes} onChange={(e) => setDailyGoals({...dailyGoals, studyMinutes: parseInt(e.target.value) || 0})} className="w-full p-3 border-2 border-gray-200 rounded-xl outline-none focus:border-indigo-500" />
                      </div>
                      
                      <div>
                        <label className="block text-sm font-medium text-gray-700 mb-2">Screen Time Limit (minutes per day)</label>
                        <input type="number" value={dailyGoals.screenTimeMinutes} onChange={(e) => setDailyGoals({...dailyGoals, screenTimeMinutes: parseInt(e.target.value) || 0}
