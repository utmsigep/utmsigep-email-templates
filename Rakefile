#!/usr/bin/env ruby

require 'dotenv'
require 'net/http'
require 'fileutils'
require 'createsend'

Dotenv.load

# Configuration
api_key = ENV['CAMPAIGN_MONITOR_API_KEY']
client_id = ENV['CAMPAIGN_MONITOR_CLIENT_ID']
templates = [
  {
    name: 'Alumni Weekend',
    id: 'c25205c55da9c98a1dfb10927800beaa',
    path: 'alumni-weekend'
  },
  {
    name: 'Balanced Man Scholarship',
    id: '38dd8e4d85d50c15106596d2faf11a5e',
    path: 'balanced-man-scholarship'
  },
  {
    name: 'Contact Information Update',
    id: 'e2b71b3dc4f951ead4564aab35db938f',
    path: 'contact-info-update'
  },
  {
    name: 'Editable Header',
    id: 'f6e5732b4c19def9682fac31986109b7',
    path: 'editable-header'
  },
  {
    name: 'Message from the AVC',
    id: '32db187f384b5bce57122fb95f735399',
    path: 'message-from-avc'
  },
  {
    name: 'Message from the Chapter',
    id: 'c76de5288f0a3e805f302b9cb53ba922',
    path: 'message-from-chapter'
  },
  {
    name: 'Newsletter (Two Column)',
    id: 'e5ae02746f8e92389225b8b03a57da3f',
    path: 'newsletter-two-column'
  },
  {
    name: 'The 1901 Club',
    id: '700ce58c276b1f224da1a924cec1d4be',
    path: '1901-club'
  }
]
baseurl = 'https://utmsigep.github.io/utmsigep-email-templates/'
basepath = Dir.pwd

task default: %w[build]

task :build do
  # Iterate through and zip assets
  templates.each do |t|
    Dir.chdir("#{basepath}/#{t[:path]}")
    system("zip -q -r #{t[:path]}-assets.zip images/* style.css")
  end
end

task :update do
  auth = { api_key: api_key }
  cs = CreateSend::CreateSend.new(auth)
  templates.each do |t|
    template = CreateSend::Template.new auth, t[:id]
    template.update(t[:name], "#{baseurl}/#{t[:path]}/index.html", "#{baseurl}/#{t[:path]}/#{t[:path]}-assets.zip")
  end
end
