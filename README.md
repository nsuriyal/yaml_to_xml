inboundCall:
  name: Ankit_test_Architect_Flow
  description: Test flow
  division: Home
  startUpRef: "/inboundCall/menus/menu[Main Menu_10]"
  defaultLanguage: en-us
  supportedLanguages:
    en-us:
      defaultLanguageSkill:
        noValue: true
      textToSpeech:
        defaultEngine:
          voice: Jill
  initialGreeting:
    exp: AudioPlaybackOptions(Append(ToAudioBlank(500), ToAudioTTS("   Thank you for calling Ankit's test service flow")), true)
  settingsActionDefaults:
    playAudioOnSilence:
      timeout:
        lit:
          seconds: 40
    detectSilence:
      timeout:
        lit:
          seconds: 40
    callData:
      processingPrompt:
        noValue: true
    collectInput:
      noEntryTimeout:
        lit:
          seconds: 5
    dialByExtension:
      interDigitTimeout:
        lit:
          seconds: 6
    transferToUser:
      connectTimeout:
        noValue: true
    transferToNumber:
      connectTimeout:
        noValue: true
    transferToGroup:
      connectTimeout:
        noValue: true
    transferToFlowSecure:
      connectTimeout:
        lit:
          seconds: 15
  settingsErrorHandling:
    errorHandling:
      disconnect:
        none: true
    preHandlingAudio:
      tts: Sorry, an error occurred. Please try your call again.
  settingsMenu:
    extensionDialingMaxDelay:
      lit:
        seconds: 1
    listenForExtensionDialing:
      lit: true
    menuSelectionTimeout:
      lit:
        seconds: 10
    repeatCount:
      lit: 3
  settingsPrompts:
    ensureAudioInPrompts: false
    promptMediaToValidate:
      - mediaType: audio
      - mediaType: tts
  settingsSpeechRec:
    completeMatchTimeout:
      lit:
        ms: 100
    incompleteMatchTimeout:
      lit:
        ms: 1500
    maxSpeechLengthTimeout:
      lit:
        seconds: 20
    minConfidenceLevel:
      lit: 50
    asrCompanyDir: none
    asrEnabledOnFlow: false
    suppressRecording: false
  menus:
    - menu:
        name: Main Menu
        refId: Main Menu_10
        audio:
          exp: AudioPlaybackOptions(ToAudioBlank(100), true)
        settingsMenu:
          extensionDialingMaxDelay:
            noValue: true
          listenForExtensionDialing:
            lit: false
          menuSelectionTimeout:
            lit:
              ms: 0
          repeatCount:
            lit: 0
        settingsSpeechRec:
          completeMatchTimeout:
            noValue: true
          incompleteMatchTimeout:
            noValue: true
          maxSpeechLengthTimeout:
            noValue: true
          minConfidenceLevel:
            noValue: true
        choices:
          - menuTransferToAcd:
              name: Transfer to ACD
              refId: Transfer to ACD_12
              dtmf: digit_1
              globalDtmf: false
              globalSpeechRecTerms: false
              targetQueue:
                lit:
                  name: Ankit_test_Queue
              acdSkills:
                - acdSkill:
                    lit:
                      name: Ankit_test_Skill
              preTransferAudio:
                tts: Please wait for the next available  agent
              failureTransferAudio:
                tts: Sorry, we were unable to complete the  transfer
              priority:
                lit: 5
              preferredAgents:
                noValue: true
              languageSkill:
                noValue: true
              failureOutputs:
                errorType:
                  noValue: true
                errorMessage:
                  noValue: true
          - menuTask:
              name: New Task 1
              dtmf: digit_3
              globalDtmf: false
              globalSpeechRecTerms: false
              task:
                actions:
                  - evaluateScheduleGroup:
                      name: Evaluate Schedule Group
                      inServiceSchedules:
                        noValue: true
                      evaluate:
                        now: true
                      scheduleGroup:
                        lit:
                          name: Ankit_Test_Schedule_Group
                        name: Ankit_Test_Schedule_Group
                      emergencyGroup:
                        noValue: true
                      outputs:
                        open:
                          actions:
                            - dataTableLookup:
                                name: Data Table Lookup
                                lookupValue:
                                  exp: Prompt.Ankit_Test_Selectlang
                                dataTable:
                                  Ankit_test_DT:
                                    foundOutputs:
                                      Queue name:
                                        noValue: true
                                      Audio:
                                        noValue: true
                                    failureOutputs:
                                      errorType:
                                        noValue: true
                                      errorMessage:
                                        noValue: true
                                outputs:
                                  found:
                                    actions:
                                      - transferToAcd:
                                          name: Transfer to ACD
                                          targetQueue:
                                            lit:
                                              name: Ankit_test_Queue
                                          acdSkills:
                                            - acdSkill:
                                                lit:
                                                  name: Ankit_test_Skill
                                          preTransferAudio:
                                            tts: Connecting you shortly
                                          failureTransferAudio:
                                            tts: Sorry all agents are busy
                                          priority:
                                            lit: 5
                                          preferredAgents:
                                            noValue: true
                                          languageSkill:
                                            noValue: true
                                          failureOutputs:
                                            errorType:
                                              noValue: true
                                            errorMessage:
                                              noValue: true
                                          outputs:
                                            failure:
                                              actions:
                                                - flushAudio:
                                                    name: Flush Audio
                                  notFound:
                                    actions:
                                      - playAudio:
                                          name: Play Audio
                                          audio:
                                            tts: Not found
                                  failure:
                                    actions:
                                      - playAudio:
                                          name: Play Audio
                                          audio:
                                            tts: Failure
                        closed:
                          actions:
                            - transferToFlow:
                                name: Transfer to Flow
                                targetFlow:
                                  name: Ankit_Closed_CF_Test
                                preTransferAudio:
                                  tts: Closed flow
                                failureTransferAudio:
                                  tts: closed flow
                                failureOutputs:
                                  errorType:
                                    noValue: true
                                  errorMessage:
                                    noValue: true
                                outputs:
                                  failure:
                                    actions:
                                      - flushAudio:
                                          name: Flush Audio
                        holiday:
                          actions:
                            - flushAudio:
                                name: Flush Audio
                        emergency:
                          actions:
                            - flushAudio:
                                name: Flush Audio
                  - disconnect:
                      name: Disconnect
        defaultChildMenuRef: "./choices/menuTransferToAcd[Transfer to ACD_12]"
