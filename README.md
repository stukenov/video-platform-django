# Video Platform Django

A modern video hosting and processing platform built with Django. Features include video upload, automatic transcoding, HLS streaming, thumbnail generation, and asynchronous task processing with Celery.

## Features

- **Video Upload & Management**: Upload videos through a user-friendly interface
- **Automatic Transcoding**: Convert videos to web-optimized formats using FFmpeg
- **HLS Streaming**: Generate HTTP Live Streaming (HLS) playlists for adaptive streaming
- **Thumbnail Generation**: Automatically create video thumbnails using OpenCV
- **Async Processing**: Background video processing with Celery and Redis
- **Progress Tracking**: Real-time transcoding progress monitoring
- **Unique Video IDs**: 6-character alphanumeric UIDs for efficient organization
- **Django Admin**: Full-featured admin interface for content management

## Tech Stack

- **Framework**: Django 4.x
- **Task Queue**: Celery with Redis broker
- **Database**: SQLite (default), PostgreSQL/MySQL compatible
- **Video Processing**: FFmpeg, OpenCV
- **Frontend**: Bootstrap 5 with Crispy Forms
- **Task Monitoring**: Real-time progress tracking

## Prerequisites

- Python 3.8+
- Redis server (for Celery)
- FFmpeg (for video processing)
- OpenCV (for thumbnail generation)

## Installation

1. Clone the repository:
```bash
git clone https://github.com/stukenov/video-platform-django.git
cd video-platform-django
```

2. Create and activate virtual environment:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

4. Configure environment variables:
```bash
cp .env.example .env
# Edit .env with your settings
```

5. Run migrations:
```bash
python manage.py migrate
```

6. Create superuser:
```bash
python manage.py createsuperuser
```

7. Collect static files:
```bash
python manage.py collectstatic
```

## Running the Application

### Development Mode

1. Start Redis server:
```bash
redis-server
```

2. Start Celery worker (in separate terminal):
```bash
celery -A qaztube worker -l info
```

3. Run Django development server:
```bash
python manage.py runserver
```

4. Access the application:
   - Main site: http://localhost:8000
   - Admin panel: http://localhost:8000/admin

### Production Deployment

For production, use Gunicorn with the included configuration:

```bash
gunicorn -c gunicorn.conf.py qaztube.wsgi:application
```

Or use PM2 with the included ecosystem config:

```bash
pm2 start ecosystem.config.js
```

## Project Structure

```
video-platform-django/
├── qaztube/              # Main Django project
│   ├── settings.py       # Django settings
│   ├── urls.py          # URL routing
│   └── wsgi.py          # WSGI application
├── videos/              # Video management app
│   ├── models.py        # Video model with transcoding
│   ├── views.py         # Video upload and display
│   ├── upload.py        # Chunked upload handler
│   ├── queue.py         # Celery tasks
│   └── templates/       # HTML templates
├── services/            # Background services
│   └── video_processor/ # Video transcoding service
├── templates/           # Global templates
├── static/              # Static files (CSS, JS)
├── media/              # User uploads (videos, thumbnails)
├── tests/              # Test suite
├── manage.py           # Django management script
└── requirements.txt    # Python dependencies
```

## Configuration

### Environment Variables

Key configuration options in `.env`:

- `SECRET_KEY`: Django secret key (change in production)
- `DEBUG`: Debug mode (False in production)
- `ALLOWED_HOSTS`: Comma-separated list of allowed hostnames
- `CELERY_BROKER_URL`: Redis URL for Celery
- `CELERY_RESULT_BACKEND`: Redis URL for task results

### Video Processing Settings

Customize video processing in `settings.py`:

```python
VIDEO_PROCESSING_FORMAT = 'mp4'
VIDEO_PROCESSING_QUALITY = 'medium'
VIDEO_PROCESSING_CRF = '23'
VIDEO_PROCESSING_AUDIO_BITRATE = '128k'
```

## Features in Detail

### Video Upload
- Chunked upload support for large files
- Progress tracking during upload
- Automatic UID generation for organization

### Transcoding Pipeline
- Automatic video format conversion
- Multiple quality options (low, medium, high)
- HLS playlist generation for streaming
- Thumbnail extraction from video frames

### Storage Structure
Videos are organized by UID in nested directories:
```
media/videos/a/b/c/1/2/3/
├── video.mp4          # Original upload
├── processed.mp4      # Transcoded version
├── thumbnail.jpg      # Video thumbnail
└── master.m3u8       # HLS playlist
```

## API Endpoints

- `GET /` - Homepage with video list
- `GET /video/<uid>/` - Video player page
- `POST /upload/` - Video upload endpoint
- `GET /admin/` - Django admin interface

## Development

### Running Tests

```bash
python manage.py test
# or with pytest
pytest
```

### Code Style

The project follows PEP 8 style guidelines.

## Troubleshooting

### Celery Not Processing Tasks
- Ensure Redis server is running
- Check Celery worker logs for errors
- Verify `CELERY_BROKER_URL` in `.env`

### FFmpeg Not Found
- Install FFmpeg: `apt-get install ffmpeg` (Ubuntu) or `brew install ffmpeg` (macOS)
- Ensure FFmpeg is in system PATH

### Video Upload Fails
- Check `MEDIA_ROOT` directory permissions
- Verify disk space availability
- Check maximum file upload size in settings

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Author

Copyright (c) 2025 Saken Tukenov

## Acknowledgments

- Built with Django and Django REST Framework
- Video processing powered by FFmpeg and OpenCV
- Task queue handled by Celery and Redis
